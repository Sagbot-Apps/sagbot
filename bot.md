# SagBot user agent: what it fetches and how to opt out

**SagBot is the fetcher behind [sagbot.app](https://sagbot.app/).** It visits a site only when someone
asks for a scan of it, reads a handful of public files, and leaves. No crawling, no schedule.

[README](README.md) · [Full page on sagbot.app](https://sagbot.app/bot)

## How do I spot SagBot in my logs?

Every request carries this User-Agent:

```http
User-Agent: SagBot/1.0 (+https://sagbot.app/bot)
```

The `robots.txt` token is `SagBot`.

## What does a scan fetch?

At most these requests, in this order. `robots.txt` comes first and can stop the scan; the rest go out together.

| Request | What it is |
|---|---|
| `GET /robots.txt` | Always the first request. If it opts SagBot out, nothing else is fetched. |
| `GET /` | Your homepage, as HTML. |
| `GET /` with `Accept: text/markdown` | Your homepage again, this time asking for Markdown. |
| `GET /sitemap.xml` | The sitemaps listed in `robots.txt` (up to three), then `/sitemap.xml`, until one loads. |
| `GET /llms.txt`, `/llms-full.txt` | Your llms.txt files, if they exist. |
| `GET /.well-known/*` | Agent discovery files such as `api-catalog`, OAuth / OpenID metadata, MCP server cards, `agent-card.json` and `ai-catalog.json`. |

If your homepage redirects to another site, SagBot scans that site instead, starting from its
homepage and honouring its `robots.txt`. Every other request stays on the scanned site: redirects
elsewhere are refused.

SagBot never follows links, submits forms, runs JavaScript or reads anything behind a login.

## When does SagBot visit?

Only when someone runs a scan. A finished scan is reused for an hour, so new scans of your site in
that hour don't reach your server. Only a manual rescan fetches again, and at most once every two
minutes.

## What are the hard limits?

- GET and HEAD requests only
- Gives up on a request after 8 seconds
- Stops reading a response after 2 MB
- Public hostnames only: no IP addresses, private networks or custom ports

## How do I block SagBot?

Add this to your `robots.txt`:

```text
User-agent: SagBot
Disallow: /
```

SagBot reads `robots.txt` before anything else, so after this it makes no other request to your
site. Anyone who tries to scan your site sees "Couldn't scan — site opted out of SagBot".

Only a group for SagBot counts. A site-wide `User-agent: *` block doesn't opt you out, because it's
usually there by accident.

## Contact

Seeing traffic you didn't expect, or have a question? Email [bot@sagbot.app](mailto:bot@sagbot.app)
with your domain and roughly when the requests happened.
