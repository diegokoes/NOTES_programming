# CONCEPTS > HTTPS

## SUMMARY

HTTPS is HTTP over TLS. It adds confidentiality, integrity, and server authentication (and optionally client authentication) to HTTP using the TLS protocol and X.509 certificates. It uses port 443 by default, negotiates protocols (h2/h3) via ALPN, and is table stakes for modern web apps, APIs, and CDNs. Enforce with HSTS to prevent protocol downgrades.

- Building blocks: [[concepts_tls|TLS]], [[concepts_pki|PKI/certificates]], SNI, ALPN, OCSP stapling, and modern ciphers with forward secrecy.
- Typical benefits: privacy on the wire, tamper protection, trusted identity, and better performance with HTTP/2 and HTTP/3.
- Related: [[concepts_security|security]], [[concepts_http|HTTP]], [[concepts_cors|CORS]], [[concepts_sop|SOP]], [[concepts_mime|MIME]].

## THEORY

HTTPS = HTTP + TLS. TLS wraps the HTTP connection so that request and response bytes are encrypted and authenticated, and the client can verify the server’s identity via certificates.

— Handshake and keys (TLS 1.3)
When a client connects, it sends a ClientHello indicating supported cipher suites and the server name (SNI). The server replies with a ServerHello and a certificate chain proving identity. Using (EC)DHE, both sides derive ephemeral session keys so only they can decrypt the traffic. TLS 1.3 reduces round trips and removes legacy ciphers; resumption (tickets/PSK) accelerates reconnects.

— Certificates and trust
The server presents a certificate chaining to a trusted root in the OS/browser store. The client verifies the chain, validity period, and that the hostname matches the certificate’s SAN. Revocation info can be delivered with OCSP stapling; Certificate Transparency logs make mis‑issuance detectable.

— What HTTPS protects (and what it doesn’t)
It provides confidentiality (eavesdroppers see gibberish), integrity (tampering is detected), and server authentication (you’re talking to the real site). It does not protect against malware on your device or a compromised server. For mutual authentication, mTLS adds client certificates.

— HSTS and mixed content
HSTS tells browsers to always use HTTPS for a domain (and optionally subdomains), preventing protocol downgrades and cookie leakage. Mixed content—loading HTTP subresources from an HTTPS page—breaks the security model; browsers block or warn, so all assets must be served via HTTPS.

— Protocol negotiation and performance
ALPN negotiates the HTTP layer (h2 or h3) during the handshake. HTTP/2 over TLS enables multiplexing and header compression; HTTP/3 over QUIC moves to UDP, improving loss recovery and handshake latency. Modern CDNs and browsers expect HTTPS to unlock these features.

— Deploying safely
Use ACME (e.g., Let’s Encrypt) to obtain and auto‑renew certificates (short lifetimes like 90 days). Prefer modern settings: TLS 1.2/1.3, AEAD ciphers (AES‑GCM/ChaCha20‑Poly1305), ECDSA keys where compatible, OCSP stapling on. For internal services, operate a private CA and consider mTLS.

## QUESTIONS

> [!tip]- What does HTTPS add over HTTP?
> Confidentiality (encryption), integrity (tamper detection), and server authentication (PKI). Optionally client authentication (mTLS). It doesn’t protect against malware on endpoints or compromised servers.

> [!tip]- How does TLS 1.3 improve latency and security?
> Fewer round trips (0-RTT/1-RTT handshakes), removal of legacy ciphers, mandatory forward secrecy via (EC)DHE, and simpler negotiation; results in faster and safer connections.

> [!warning]- How do SNI and ALPN help?
> SNI lets multiple certificates/hosts share an IP by indicating the hostname in the ClientHello. ALPN negotiates the application protocol (e.g., h2, h3) during TLS, avoiding extra round trips.

> [!warning]- When should I use mTLS?
> Use for service-to-service auth or high-security B2B scenarios. Manage client certs via a private CA, short lifetimes, automated issuance/rotation, and revocation (CRL/OCSP).

> [!warning]- What does HSTS do and what about preload?
> HSTS forces HTTPS for a host for a duration; prevents downgrade/stripping. Preload ships the policy in browsers—powerful but risky if misconfigured (can lock out HTTP accidentally).

> [!danger]- How do OCSP stapling and CT logs help?
> OCSP stapling provides fresh revocation status with the handshake, improving privacy and reliability. Certificate Transparency logs make mis-issuance detectable; many clients require SCTs.

> [!danger]- What is mixed content and how to fix it?
> Loading HTTP resources on an HTTPS page; browsers block or warn. Fix by serving all subresources over HTTPS or upgrading URLs (use protocol-relative or https:// explicitly).

> [!danger]- Key/cipher/cert best practices?
> Use TLS 1.2/1.3 only, prefer ECDSA with P-256, enable AEAD ciphers (AES-GCM/ChaCha20-Poly1305), rotate certs ~90 days (ACME), enable OCSP stapling, and strong DH params.

> [!danger]- How do CDNs handle TLS to origin?
> They terminate TLS at the edge and may re-encrypt to the origin (origin pull TLS) or use private networking. Ensure origin cert validation and SNI; avoid "Flexible" modes that leave origin in clear.
