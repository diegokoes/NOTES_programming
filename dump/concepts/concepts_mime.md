# CONCEPTSPROTOCOLS > MIME

## SUMMARY
>
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
- Registration and governance
  - IANA maintains the registry of official media types.
  - New types follow RFC 6838: specification, template, review, and registration process.
- Detection vs declaration
  - Declaration: The sender sets an authoritative `Content-Type`.
  - Detection: Receivers may inspect “magic numbers” or use “MIME sniffing” when headers are missing/incorrect (web security risk).
- Desktop/XDG (Linux)
  - Shared MIME-info database maps magic bytes and file globs to types.
  - Tooling: `xdg-mime query filetype file.ext`, set defaults via `xdg-mime default app.desktop application/pdf`.
  - App associations live in `mimeapps.list`; MIME definitions in `*.xml` per the spec.
- Security considerations
  - Never rely on file extensions alone; set accurate `Content-Type`.
  - Use `X-Content-Type-Options: nosniff` to prevent content sniffing bypasses.
  - Be careful with active content (e.g., `image/svg+xml` can contain scripts); sanitize or force download.

References

- IANA Media Types: <https://www.iana.org/assignments/media-types/media-types.xhtml>
- MDN: MIME types overview: <https://developer.mozilla.org/docs/Web/HTTP/Basics_of_HTTP/MIME_types>
- MDN: Content-Type: <https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Type>
- MDN: Accept: <https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept>
- MDN: Content-Disposition: <https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Disposition>
- WHATWG MIME Sniffing Standard: <https://mimesniff.spec.whatwg.org/>
- RFC 2045/2046 (MIME for email): <https://www.rfc-editor.org/rfc/rfc2045> and <https://www.rfc-editor.org/rfc/rfc2046>
- RFC 6838 (Media Type Specifications and Registration): <https://www.rfc-editor.org/rfc/rfc6838>
- RFC 6839 (Structured Syntax Suffixes): <https://www.rfc-editor.org/rfc/rfc6839>
- XDG Shared MIME-info spec: <https://specifications.freedesktop.org/shared-mime-info-spec/shared-mime-info-spec-latest.html>
- xdg-mime man page: <https://manpages.debian.org/unstable/xdg-utils/xdg-mime.1.en.html>

## QUESTIONS
>
> [!tip]- What does “MIME” stand for and what problem does it solve?
> Multipurpose Internet Mail Extensions; a standard way to label and structure content so receivers know how to interpret it.

> [!tip]- What is the general syntax of a MIME type?
> type/subtype with optional parameters, e.g., `text/html; charset=utf-8`.

> [!tip]- Which HTTP header declares the body’s MIME type?
> `Content-Type`.

> [!tip]- What does `application/octet-stream` mean?
> Generic binary data (unknown or arbitrary bytes).

> [!tip]- Where are official media types registered?
> IANA Media Types registry.

> [!tip]- Give two examples of common MIME types.
> `application/json`, `image/png`.

> [!warning]- What does the `charset` parameter do for `text/*` types?
> It declares character encoding (e.g., UTF‑8) so text is decoded correctly.

> [!warning]- What is a structured syntax suffix like `+json` used for?
> To indicate the underlying syntax, enabling generic tooling; e.g., treat `application/vnd.foo+json` as JSON.

> [!warning]- How do `Content-Type` and `Accept` differ?
> `Content-Type` describes the payload being sent; `Accept` lists formats the client can receive (for negotiation).

> [!warning]- How are MIME multipart bodies separated?
> With a unique `boundary` parameter; each part is delimited by `--boundary`.

> [!warning]- When should you use the vendor tree (`vnd.`)?
> For vendor-specific types not suitable for the standards tree, e.g., `application/vnd.acme.widget+json`.

> [!warning]- How do browsers behave if `Content-Type` is missing or wrong?
> They may sniff the type; this can be dangerous. Mitigate with accurate types and `X-Content-Type-Options: nosniff`.

> [!danger]- How would you choose a MIME type for a JSON API with a custom schema?
> Prefer `application/json` (optionally with `profile` param), or a vendor type like `application/vnd.example.resource+json` if negotiation/versioning needs justify it.

> [!danger]- Explain how XDG determines a file’s MIME type on Linux.
> It consults the shared MIME-info DB using magic bytes and filename globs with priorities; `xdg-mime query filetype path` shows the result.

> [!danger]- How do you set the default app for PDFs on Linux?
> `xdg-mime default <app.desktop> application/pdf` (e.g., `xdg-mime default org.gnome.Evince.desktop application/pdf`).

> [!danger]- Why can serving SVGs inline be risky, and how to mitigate?
> SVG (XML) can include scripts/active content; sanitize, set correct `Content-Type`, use CSP, consider `Content-Disposition: attachment`, and `nosniff`.

> [!danger]- Compare `multipart/form-data` and `application/x-www-form-urlencoded`.
> Both encode form data; `multipart/form-data` supports files/binaries via parts and boundaries, while `x-www-form-urlencoded` URL-encodes key/value pairs.

> [!danger]- What are the steps to register a new media type with IANA?
> Provide a spec and registration template per RFC 6838, undergo expert review, then IANA records the type.

- - -
