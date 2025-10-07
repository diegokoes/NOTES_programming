# CONCEPTS > ORIGIN

## SUMMARY
> [!summary]
> In websites, the origin is defined by:
> - scheme (protocol)
> - hostname (domain)
> - port
## THEORY
These are same origin because they have the same scheme (`http`) and hostname (`example.com`), and the different file path does not matter:

- `http://example.com/app1/index.html`
- `http://example.com/app2/index.html`

These are same origin because a server delivers HTTP content through port 80 by default:

- `http://example.com:80`
- `http://example.com`

These are not same origin because they use different schemes:

- `http://example.com/app1`
- `https://example.com/app2`

These are not same origin because they use different hostnames:

- `http://example.com`
- `http://www.example.com`
- `http://myapp.example.com`

These are not same origin because they use different ports:

- `http://example.com`
- `http://example.com:8080`
## QUESTIONS
> [!tip]- Question
> Answer

> [!warning]- Question
> Answer

> [!danger]- Question
> Answer

