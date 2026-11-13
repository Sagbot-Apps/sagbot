# SagBot user agent: what it fetches and how to opt out

**SagBot is the fetcher behind [sagbot.app](https://sagbot.app/).** It visits a site when someone
asks for a scan of it or runs a free tool on it, or when the site's verified owner has turned on monitoring. It reads
a handful of public files (the llms.txt generator: the `<head>` of up to 50 pages) and leaves. No crawling.

[README](README.md) · [Full page on sagbot.app](https://sagbot.app/bot)

## How do I spot SagBot in my logs?

Every request carries this User-Agent:

```http
User-Agent: SagBot/1.0 (+https://sagbot.app/bot)
```

The `robots.txt` token is `SagBot`.

Each request is also signed with [Web Bot Auth](https://developers.cloudflare.com/bots/reference/bot-verification/web-bot-auth/)
(HTTP message signatures, Ed25519): `Signature-Agent: "https://sagbot.app"` plus `Signature-Input` and
`Signature` headers. The public key is at
[`https://sagbot.app/.well-known/http-message-signatures-directory`](https://sagbot.app/.well-known/http-message-signatures-directory),
so you can check a request really comes from SagBot.

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

When someone runs a scan or a free tool, and, for monitored sites, on a schedule (see below). A finished scan is reused for an hour, so new scans of your site in
that hour don't reach your server. Only a manual rescan fetches again, and at most once every two
minutes.

## Scheduled checks

SagBot visits a site on a schedule only if its owner has verified the site and turned monitoring on. Every other
site gets no scheduled visits. A monitored site gets:

- an hourly light check (Instant monitoring): three requests (plus any redirects), for `/robots.txt`, `/llms.txt` and the homepage, to
  see whether any of them changed;
- a full scan daily, weekly or when the light check sees a change, about 25 to 30 requests each.

A `robots.txt` that opts SagBot out (see below) limits both to a single `robots.txt` request: SagBot reads it first and fetches nothing else.

## What do the free tools do?

[Free tools](https://sagbot.app/tools) that check your site's own files (`llms.txt`, AI bots, Schema) run
a normal scan. Other tools read your `robots.txt` first. If it opts SagBot out, the tool stops and doesn't
send your domain anywhere. Otherwise it asks an outside service. Some of those services test your site
from their own servers, so those visits come from them, not from SagBot.

The [Open Graph preview](https://sagbot.app/tools/open-graph-preview) is different: SagBot itself fetches
the page you enter (any path, not only the homepage) and the image its `og:image` tag points to. Before it
requests anything from a host, including every host a redirect leads to, it reads that host's `robots.txt`,
and an opted-out host is not fetched. Its results are not cached, so a check after you fix your tags shows
the new state.

The [robots.txt validator](https://sagbot.app/tools/robots-txt-validator) fetches only `/robots.txt` of
the domain you enter, live and without a cache, so you can re-check right after an edit. Because that file
is where an opt-out lives, it is read even for opted-out sites, and nothing else is fetched. A file you paste
instead is checked in your browser and never sent to SagBot.

The [llms.txt generator](https://sagbot.app/tools/llms-txt-generator) reads your `robots.txt` first: an
opt-out stops it, and if `robots.txt` can't be read it uses only the homepage. Otherwise it reads your
sitemap (or, without one, the links on your homepage) and then the `<head>` of up to 50 pages on the same
host (and its `www.` twin), 6 at a time, skipping paths your `robots.txt` blocks for SagBot. It never reads a
page body or other subdomains. The draft is kept for an hour, so repeat requests don't fetch your site again.

## What are the hard limits?

- GET and HEAD requests only
- Gives up on a request after 8 seconds
- Stops reading a response after 2 MB (Open Graph preview images: counted up to 8 MB, at most 2 MB kept)
- Public hostnames only: no IP addresses, private networks or custom ports

## How do I block SagBot?

Add this to your `robots.txt`:

```text
User-agent: SagBot
Disallow: /
```

SagBot reads `robots.txt` before anything else, so after this it makes no other request to your
site. Anyone who tries to scan your site sees "Couldn't scan — site opted out of SagBot".

SagBot also follows a site-wide `User-agent: *` group: if it disallows `/`, SagBot stays out. To let
SagBot in anyway, give it its own group:

```text
User-agent: SagBot
Allow: /
```

## Contact

Seeing traffic you didn't expect, or have a question? Email [bot@sagbot.app](mailto:bot@sagbot.app)
with your domain and roughly when the requests happened.
