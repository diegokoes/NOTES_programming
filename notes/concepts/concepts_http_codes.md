# CONCEPTS > HTTP CODES

— Status codes: what the numbers mean
Codes are grouped by the first digit:
• 1xx Informational: the request is being processed (rare in browsers; 100 Continue is common with large bodies).
• 2xx Success: the request succeeded. 200 OK returns a body; 201 Created indicates creation and usually includes a Location header; 204 No Content indicates success with no body.
• 3xx Redirection: the client should use another URL. 301/308 are permanent; 302/307 are temporary. 307/308 preserve the HTTP method and body across the redirect; 301/302 may not.
• 4xx Client errors: something about the request is wrong. 400 Bad Request (malformed), 401 Unauthorized (requires auth), 403 Forbidden (recognized but not allowed), 404 Not Found, 405 Method Not Allowed, 409 Conflict (state conflict), 422 Unprocessable Content (validation errors), 429 Too Many Requests (rate limiting, often with Retry-After).
• 5xx Server errors: the server failed to fulfill a valid request. 500 Internal Server Error, 502 Bad Gateway (upstream failure), 503 Service Unavailable (temporary overload/maintenance; may include Retry-After), 504 Gateway Timeout.

304 Not Modified is special: it’s a success for caches. The server tells the client its cached representation is still valid, so no body is sent.


