# Backend API Design Lab

A practical Java backend reference for designing clean, reliable and maintainable REST APIs.

## Focus
- REST resource design
- HTTP methods and status codes
- Request/response DTOs
- Validation
- Pagination and sorting
- Idempotency
- Global exception handling
- API versioning
- OpenAPI documentation
- Correlation IDs
- Timeouts and retry considerations
- Secure API design

## Typical flow
```text
Client -> Controller -> Validation -> Service -> DAO/Repository -> Database
                                      |
                                      v
                                   DTO Response
```

## Interview essentials
1. Why use DTOs instead of exposing persistence entities?
2. When should an API return 400, 401, 403, 404 or 409?
3. What makes an operation idempotent?
4. Why is pagination important for large datasets?
5. How should validation and exception handling be separated?
6. How do correlation IDs help production troubleshooting?
7. Why should retries use timeouts, backoff and idempotency?

This is independent learning/demo work and contains no employer or client code.