# CONCEPTS > HTTP

## SUMMARY

> [!summary]
>HTTP is a stateless, application-layer, request–response protocol used by browsers, APIs, and services to transfer representations of resources identified by URLs. It runs over TCP for HTTP/1.1 and HTTP/2, and over QUIC (UDP) for HTTP/3. Core ideas: methods (GET/POST/PUT/PATCH/DELETE), status codes (2xx–5xx), headers, bodies, caching, and content negotiation. Security is not provided by HTTP itself—use [[concepts_https|HTTPS]] for encryption and authentication.

>- Layering: Application protocol on top of [[concepts_protocols|TCP/QUIC]]; resolves names via [[concepts_dns|DNS]].
>- Typical ports: 80 (HTTP), 443 for [[concepts_https|HTTPS]].
>- Common uses: Web pages, [[concepts_rest|REST]] APIs, file transfer, streaming, and service-to-service communication.
>- Related: [[concepts_cors|CORS]], [[concepts_mime|MIME types]], [[concepts_sop|Same-Origin Policy]], [[concepts_security|Web security basics]].

## THEORY

HTTP is a stateless request–response protocol: a client sends a request message to a server, and the server replies with a response message. Stateless means the server doesn’t remember prior requests by default—any needed context must be sent again (cookies, tokens) or looked up by the server.

Request and response are plain messages with a start line, headers, and an optional body.

Example request and response:

```http
GET /items?tag=css HTTP/1.1
Host: example.com
Accept: text/html,application/xhtml+xml
If-None-Match: "abc123"

```

```http
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length: 1234
Cache-Control: max-age=3600

<html>...</html>
```

— Request line, headers, body
The request line contains the method (e.g., GET), the target (path and query), and the protocol version. Headers are name–value pairs that carry metadata (what formats you accept, credentials, cache validators, etc.). A body is optional and used when sending data (e.g., POST JSON). The response starts with a status line (version + code + reason), followed by headers describing the representation (e.g., Content-Type) and optional body bytes.

— Methods and their semantics
Methods express intent:
• GET fetches a representation; it’s safe (shouldn’t change server state) and idempotent (repeating has same effect).
• HEAD is like GET but returns headers only.
• POST submits data to be processed (create, command, or action). Not guaranteed idempotent.
• PUT replaces a resource’s representation at a target URL; idempotent.
• PATCH applies a partial modification; not guaranteed idempotent.
• DELETE removes a resource; idempotent if repeated deletes have no further effect.
• OPTIONS describes communication options (also used by browsers for CORS preflight).

Safe vs idempotent matters for intermediaries and retries: idempotent methods can be retried automatically with lower risk.

— Status codes: what the numbers mean
Codes are grouped by the first digit:
• 1xx Informational: the request is being processed (rare in browsers; 100 Continue is common with large bodies).
• 2xx Success: the request succeeded. 200 OK returns a body; 201 Created indicates creation and usually includes a Location header; 204 No Content indicates success with no body.
• 3xx Redirection: the client should use another URL. 301/308 are permanent; 302/307 are temporary. 307/308 preserve the HTTP method and body across the redirect; 301/302 may not.
• 4xx Client errors: something about the request is wrong. 400 Bad Request (malformed), 401 Unauthorized (requires auth), 403 Forbidden (recognized but not allowed), 404 Not Found, 405 Method Not Allowed, 409 Conflict (state conflict), 422 Unprocessable Content (validation errors), 429 Too Many Requests (rate limiting, often with Retry-After).
• 5xx Server errors: the server failed to fulfill a valid request. 500 Internal Server Error, 502 Bad Gateway (upstream failure), 503 Service Unavailable (temporary overload/maintenance; may include Retry-After), 504 Gateway Timeout.

304 Not Modified is special: it’s a success for caches. The server tells the client its cached representation is still valid, so no body is sent.

— Representation and negotiation
The response body is a representation of a resource. Content-Type (a [[concepts_mime|MIME type]]) tells the client how to interpret it; Content-Language and Content-Encoding add detail. Clients can influence what they get using Accept, Accept-Language, and Accept-Encoding. Servers pick the best match and may signal it with Vary so caches build correct keys.

— Caching, freshness, and validation
Freshness says “you may reuse this response without recontacting the origin” and is controlled by Cache-Control (max-age, s-maxage, public/private) and Expires. When a response becomes stale, clients may validate it using validators: strong ETag (exact match) or Last-Modified timestamps. If the server answers 304 Not Modified to If-None-Match/If-Modified-Since, the client can reuse its cached body. Weak ETags (W/"...") allow change-tolerant comparisons. Vary declares which request headers are part of the cache key. Some directives like must-revalidate, no-store, and stale-while-revalidate change behavior for reliability and privacy.

— Message framing and bodies
For HTTP/1.1, Content-Length indicates body size; when the size isn’t known in advance, Transfer-Encoding: chunked streams the body in chunks. In HTTP/2 and HTTP/3, framing is handled by the protocol, so chunked is not used; the semantics of methods and status codes remain the same.

— Connections and versions
HTTP/1.1 reused TCP connections with keep-alive but could only have one in-flight request per connection; pipelining had head‑of‑line blocking issues. HTTP/2 introduced binary framing, header compression, and multiplexing many streams over a single TCP connection. HTTP/3 runs over QUIC (UDP), keeping multiplexing even when packets are lost and integrating TLS 1.3 for faster setup. Despite these transport changes, HTTP’s semantics—methods, status codes, headers—do not change.

— URLs, query parameters, and request bodies
URLs identify resources and may include a query string for selection/filtering. Query parameters are visible and easily cached/bookmarked. When sending larger or state‑changing data, use a request body (e.g., JSON for APIs or multipart for file upload). Servers should define clearly which fields go where.

## QUESTIONS

> [!tip]- What does “stateless” mean in HTTP?
> The server does not retain client context between requests. Each request must carry all information needed (URL, headers, body). State usually lives on the client (cookies, tokens) or in server storage keyed by a session identifier—HTTP semantics remain stateless.

> [!tip]- Which methods are safe and which are idempotent?
> Safe: GET, HEAD, OPTIONS, TRACE (no server state change). Idempotent: GET, HEAD, OPTIONS, PUT, DELETE (same request repeated → same effect). This matters for retries and caching behavior; POST/PATCH are not guaranteed idempotent.

> [!tip]- When should I use query parameters vs. a request body?
> Query parameters are for identification/filtering and are visible/bookmarkable; bodies (POST/PUT/PATCH) carry larger or state-changing data. Query length limits are implementation-dependent; bodies avoid those limits and can be structured (JSON, multipart, forms).

> [!warning]- How do HTTP/1.1, HTTP/2, and HTTP/3 differ?
> HTTP/1.1 is text-based with no multiplexing (keep-alive helps reuse). HTTP/2 adds binary framing, multiplexing, and HPACK header compression over TCP. HTTP/3 runs over QUIC/UDP, integrating TLS 1.3 and avoiding TCP head‑of‑line blocking.

> [!warning]- ETag vs. Last-Modified for cache validation?
> ETag is an opaque content fingerprint—precise even if timestamps don’t change; Last-Modified is a timestamp with coarse granularity and clock issues. Many servers send both; clients can use If-None-Match or If-Modified-Since to get 304 Not Modified.

> [!warning]- 301/302 vs. 307/308 redirects—what’s the difference?
> 301/302 may change the method to GET in some clients; 307/308 preserve the original method and body. Prefer 308 for permanent redirects that must keep the method; use 307 for temporary ones.

> [!danger]- How do Cache-Control and Vary affect CDNs and proxies?
> Cache-Control (max-age, s-maxage, private/public, must-revalidate, stale-while-revalidate) governs freshness. Vary alters the cache key (e.g., Vary: Accept-Encoding, Accept-Language). Misuse can cause cache misses or leaks across user variants.

> [!danger]- How do [[concepts_sop|SOP]] and [[concepts_cors|CORS]] interact?
> SOP limits a page’s access to resources from other origins. CORS opts a server into allowing cross-origin requests via headers (Access-Control-Allow-*). Credentialed requests require Allow-Credentials: true and cannot use wildcard origins.

> [!danger]- Why use [[concepts_https|HTTPS]] even for “non‑sensitive” content?
> It prevents tampering/injection, hides URLs/queries from intermediaries, thwarts passive tracking, and enables modern features (Service Workers, HTTP/2/3). Mixed content risks are eliminated when everything is served over HTTPS.
