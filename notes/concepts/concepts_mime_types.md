# CONCEPTSPROTOCOLS > MIME TYPES

## SUMMARY

> [!summary]
> ==Multipurpose Internet Mail Extensions== is an internet **standard**  that labels the nature and format of data using type/subtype tokens (e.g., `text/html`, `image/png`).
>
> MIME types travel in protocols like HTTP and email to tell receivers how to parse, display, or handle content.

## THEORY

- Core idea
  - A MIME type is a case-insensitive type/subtype identifier with optional parameters: `type/subtype; param=value`.
  - Examples: `text/plain; charset=utf-8`, `application/json`, `image/svg+xml`.
  - Parameters convey extra parsing info (e.g., `charset`, `boundary`, `profile`).
- Where they’re used
  - Email: MIME enables rich content and attachments (multipart, encodings).
  - Web/HTTP: Servers set `Content-Type`; clients send `Accept` to negotiate formats.
  - Desktops: OSes and desktop environments map files to applications via MIME databases.
- Important headers (HTTP)
  - Content-Type: Declares the media type of the payload.
  - Accept: Lists preferred response media types (with q-weights) for content negotiation.
  - Content-Disposition: Suggests inline display or download (`attachment; filename="..."`).
  - X-Content-Type-Options: `nosniff` tells browsers not to guess types (security).
- Common types and conventions
  - Generic binary: `application/octet-stream` (unknown or arbitrary binary data).
  - Structured syntax suffixes: `+json`, `+xml`, `+ber`, etc. (e.g., `application/hal+json`).
  - Multipart: `multipart/mixed`, `multipart/related`, `multipart/form-data` with a `boundary` parameter.
  - Trees and naming (RFC 6838): Standard, vendor (`vnd.`), personal (`prs.`), and obsolete `x-` prefix.
### Common MIME Types

| Pattern | Matches |
|---------|---------|
| `text/plain` | `.txt` files |
| `text/html` | `.html` files |
| `text/markdown` | `.md` files |
| `image/jpeg` | `.jpg`, `.jpeg` |
| `image/png` | `.png` |
| `video/mp4` | `.mp4` |
| `video/x-matroska` | `.mkv` |
| `audio/mpeg` | `.mp3` |
| `application/pdf` | `.pdf` |
| `application/zip` | `.zip` |
| `inode/x-empty` | Empty files |


## QUESTIONS

> [!tip]- What does `application/octet-stream` mean?
> Generic binary data (unknown or arbitrary bytes).

> [!warning]- What does the `charset` parameter do for `text/*` types?
> It declares character encoding (e.g., UTF‑8) so text is decoded correctly.

> [!warning]- What is a structured syntax suffix like `+json` used for?
> To indicate the underlying syntax, enabling generic tooling; e.g., treat `application/vnd.foo+json` as JSON.

> [!warning]- How do `Content-Type` and `Accept` differ?
> `Content-Type` describes the payload being sent; `Accept` lists formats the client can receive (for negotiation).

> [!warning]- How are MIME multipart bodies separated?
> With a unique `boundary` parameter; each part is delimited by `--boundary`.

> [!warning]- How do browsers behave if `Content-Type` is missing or wrong?
> They may sniff the type; this can be dangerous. Mitigate with accurate types and `X-Content-Type-Options: nosniff`.

> [!danger]- Why can serving SVGs inline be risky, and how to mitigate?
> SVG (XML) can include scripts/active content; sanitize, set correct `Content-Type`, use CSP, consider `Content-Disposition: attachment`, and `nosniff`.
> [!danger]- Compare `multipart/form-data` and `application/x-www-form-urlencoded`.
> Both encode form data; `multipart/form-data` supports files/binaries via parts and boundaries, while `x-www-form-urlencoded` URL-encodes key/value pairs.
