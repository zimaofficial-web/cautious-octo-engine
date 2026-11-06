# Backend Engineering Standards

## 1. Server Architecture & API Design

### 1.1 RESTful API Principles
- **Resource-Oriented URIs:** Use clear, plural nouns (`/api/v1/users`, `/api/v1/orders/{id}/items`). Never use verbs in paths (avoid `/api/getUser` or `/api/createOrder`).
- **Standard HTTP Methods:**
  - `GET`: Safe, idempotent data retrieval. No side effects.
  - `POST`: Non-idempotent resource creation or action execution.
  - `PUT`: Complete idempotent resource replacement.
  - `PATCH`: Partial resource update.
  - `DELETE`: Idempotent resource removal.
- **Consistent Status Codes:**
  - `200 OK`: Successful retrieval or update.
  - `201 Created`: Successful creation (include `Location` header or created object).
  - `204 No Content`: Successful execution with no return body (common for DELETE).
  - `400 Bad Request`: Validation failure or malformed payload.
  - `401 Unauthorized`: Missing or invalid authentication token.
  - `403 Forbidden`: Authenticated user lacks permission to access resource.
  - `404 Not Found`: Resource does not exist.
  - `409 Conflict`: Business logic collision (e.g., unique email already registered).
  - `422 Unprocessable Entity`: Semantic validation failure.
  - `429 Too Many Requests`: Rate limit exceeded (include `Retry-After`).
  - `500 Internal Server Error`: Unhandled server-side failure.

### 1.2 Pagination, Sorting & Filtering
- Never return unbounded database queries (`SELECT * FROM table`).
- Standardize pagination on either:
  - **Cursor-based (Recommended for scale & feeds):** `?limit=25&cursor=eyJpZCI6MTIzfQ`
  - **Offset-based (Simple tables):** `?page=1&limit=25` (Enforce `maxLimit = 100`).
- Return pagination metadata (`has_more`, `next_cursor`, `total_count`).

---

## 2. Request Handling & Middleware Pipeline

Every HTTP request must pass through an orderly pipeline:
```text
Request Ingress
  │
  ├── 1. Request ID / Correlation ID Middleware (Attach UUID to context & response headers)
  ├── 2. Security Headers (Helmet, CORS validation)
  ├── 3. Rate Limiter (IP / User token bucket)
  ├── 4. Authentication Middleware (Verify JWT/Session, attach User to request context)
  ├── 5. Payload Validation Middleware (Parse body/query with Zod/Joi, reject malformed before handlers)
  ├── 6. Authorization Guard (Check RBAC/ABAC permissions for the target endpoint)
  │
  ├── [ Route Controller / Handler Execution ]
  │
  └── 7. Global Error Handler (Catch uncaught exceptions, format standard JSON error response)
```

---

## 3. Input Validation & Defense-in-Depth

- **Zero-Trust Ingress:** Every input (query params, path params, headers, JSON body) must be parsed by a strict validation schema before reaching business services.
- **Strict Parsing:** Reject unknown properties (`strip` or `strict` mode) to prevent mass-assignment vulnerabilities.
- **Sanitization:** Strip dangerous control characters, normalize emails (`toLowerCase().trim()`), and enforce bounded string lengths.

---

## 4. Error Management & Standard Error Envelope

### 4.1 Structured Error Response
All API errors must follow an RFC 7807-style standard JSON envelope:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The provided input data failed validation.",
    "correlation_id": "c73a2b40-9e8c-4b5a-9321-df6288349a12",
    "timestamp": "2026-11-06T20:30:00Z",
    "details": [
      {
        "field": "email",
        "issue": "Must be a valid email address"
      }
    ]
  }
}
```

### 4.2 Security Rules for Errors
- **Never leak stack traces:** Stack traces, internal file paths, and database query strings must NEVER be returned in production responses.
- **Log Internally, Sanitize Externally:** Log the full stack trace and request metadata internally against the `correlation_id`, but present a sanitized, friendly message to the client.

---

## 5. Structured Logging Standards

- **Machine-Readable JSON:** Logs must be emitted as single-line JSON objects to stdout/stderr.
- **Mandatory Fields:**
  - `level`: `debug` | `info` | `warn` | `error` | `fatal`
  - `timestamp`: ISO 8601 UTC string.
  - `correlation_id`: Matches the incoming request ID.
  - `message`: Clear human-readable description.
  - `context`: Structured key-value object containing domain identifiers (`userId`, `orderId`).
- **Strict PII Redaction:** Never log sensitive personal or financial information:
  - Redact passwords, credit card numbers, auth tokens, session secrets, and personal identity documents.

---

## 6. Background Jobs & Asynchronous Processing

- **Offload Heavy Work:** Never perform email sending, report generation, image resizing, or external webhooks inside the synchronous HTTP request/response cycle.
- **Durable Message Queues:** Use reliable queues (BullMQ, Redis, SQS, RabbitMQ).
- **Idempotency:** Workers must be designed to safely process the same message multiple times without duplicate side effects.
- **Dead-Letter Queues (DLQ):** Failed jobs must retry with exponential backoff (e.g., 3–5 attempts). Jobs failing all retries must land in a DLQ for investigation and alerting.

