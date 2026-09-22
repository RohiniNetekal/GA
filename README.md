# Backend API Engineering Lab

A hands-on Java backend repository focused on designing REST APIs that are maintainable, predictable, secure and production-friendly.

> Independent learning/demo work. No employer or client source code is included.

## Request-to-response architecture

~~~text
Client
  |
  v
Controller -> Validation -> Service -> DAO/Repository -> Database
  |                         |
  +---- Response DTO <-------+
~~~

## Resource design

Typical API examples:

~~~text
GET    /api/v1/merchants/101
GET    /api/v1/merchants?page=0&size=20
POST   /api/v1/merchants
PUT    /api/v1/merchants/101
PATCH  /api/v1/merchants/101/status
DELETE /api/v1/merchants/101
~~~

The contract should define request fields, response fields, validation rules, authentication, status codes, pagination and error behavior.

## DTOs

A database entity represents persistence state. A DTO represents the API contract.

Benefits:
- Prevents accidental exposure of internal fields
- Keeps database and API models loosely coupled
- Makes response shaping explicit
- Helps API versioning

## HTTP status reasoning

| Status | Meaning |
|---|---|
| 200 | Successful operation |
| 201 | Resource created |
| 204 | Success with no response body |
| 400 | Invalid request |
| 401 | Authentication missing/invalid |
| 403 | Authenticated but not authorized |
| 404 | Resource not found |
| 409 | State/conflict problem |
| 500 | Unexpected server failure |

## Validation vs business rules

Input validation checks whether the request is structurally valid.

Example:
- name must not be blank
- amount must be positive
- status must be one of the allowed values

Business validation checks application rules.

Example:
- A merchant cannot be activated before onboarding is complete.

Keeping these concerns separate makes services easier to test.

## Global exception handling

A common error contract can contain:

~~~json
{
  "code": "MERCHANT_NOT_FOUND",
  "message": "Merchant was not found",
  "path": "/api/v1/merchants/101",
  "correlationId": "7f3c..."
}
~~~

Do not expose stack traces, SQL details or internal infrastructure information to API consumers.

## Idempotency

For payment/order-style APIs, clients may retry after a timeout without knowing whether the first request succeeded.

A typical approach is:

~~~text
Client
  |
  | Idempotency-Key: abc123
  v
API
  |
  +--> key exists? --> return previous result
  |
  +--> new request --> process + persist result
~~~

The important part is that the key and business result must be persisted safely.

## Pagination

Large tables should not be returned without bounds.

~~~text
GET /api/v1/transactions?page=0&size=50&sort=createdAt,desc
~~~

The server should enforce a maximum page size.

## Reliability

Production APIs commonly need:
- Connection/read timeouts
- Safe retries
- Exponential backoff
- Idempotency
- Rate limiting
- Correlation IDs
- Structured logging
- Metrics and tracing

A retry without a timeout can wait indefinitely. A retry without idempotency can duplicate a business operation.

## Interview questions

1. Why use DTOs instead of exposing JPA entities?
2. Difference between 401 and 403?
3. How would you make a POST payment operation retry-safe?
4. Why is pagination necessary?
5. Where should business rules live?
6. How would you design a standard error response?
7. When should an API return 409 instead of 400?

## Planned implementation

CRUD API -> validation -> exception handling -> pagination -> OpenAPI -> idempotency -> security -> integration tests.