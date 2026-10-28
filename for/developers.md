# AI readiness for developers: robots.txt, llms.txt and Markdown for agents

**Can AI crawlers and agents reach and read your site?** Before an AI answer can quote you or an
agent can act on your site, its bot has to get in and find something it can parse. That comes down
to a few files and headers you own: `robots.txt`, a sitemap, `llms.txt`, Markdown on request and
`Link` headers. Each one is a small change.

[Check your site with SagBot](https://sagbot.app/) · [Full page on sagbot.app](https://sagbot.app/for/developers) · [README](../README.md#who-is-sagbot-for)

## Checklist

| Do this | SagBot check |
|---|---|
| Serve a `robots.txt` that parses | `agent-robots` |
| Let AI search and assistant bots in | `agent-ai-bots` |
| Point bots at a sitemap | `agent-sitemap` |
| Publish an `llms.txt` | `agent-llms-txt` |
| Answer `Accept: text/markdown` with Markdown | `agent-markdown` |
| Send `Link` headers | `agent-link-headers` |
| If you run bots, publish a key directory | `agent-web-bot-auth` (advice) |

## Serve a robots.txt that parses

Every crawler and AI agent asks for `/robots.txt` before anything else. A missing file is fine, but
one that returns HTML from your app shell, or a 5xx, leaves bots guessing what they may fetch.
Return plain text with a 200, one `User-agent` group per rule set, and a `Sitemap:` line.

```text
# Opt out of one training crawler, allow everything else
User-agent: GPTBot
Disallow: /

User-agent: *
Allow: /

Sitemap: https://example.com/sitemap.xml
```

Quick test:

```bash
curl -sI https://example.com/robots.txt | grep -i -E '^(HTTP|content-type)'
# want: HTTP/2 200 and content-type: text/plain
```

**SagBot reads** your `/robots.txt`: the status code, the content type and whether its rules parse.

## Let AI search and assistant bots in

Blocking `GPTBot` to keep your content out of training is a policy choice. Blocking `OAI-SearchBot`,
`PerplexityBot` or `ChatGPT-User` is usually an accident, and it means your site can't be quoted or
linked in their answers. Check that a broad `Disallow: /` for `*` doesn't catch the search and user
agents you meant to allow.

The 24 AI bots SagBot checks, by what they do:

| Operator | Training (opt-out is free) | Search | User-triggered |
|---|---|---|---|
| OpenAI | `GPTBot` | `OAI-SearchBot` | `ChatGPT-User` |
| Anthropic | `ClaudeBot` | `Claude-SearchBot` | `Claude-User` |
| Perplexity | | `PerplexityBot` | `Perplexity-User` |
| Google | `Google-Extended` | | `Google-Agent` |
| Apple | `Applebot-Extended` | `Applebot` | |
| Meta | `meta-externalagent` | `meta-webindexer` | `meta-externalfetcher` |
| Amazon | `Amazonbot` | `Amzn-SearchBot` | `Amzn-User` |
| Mistral | `MistralAI-Training` | `MistralAI-Index` | `MistralAI-User` |
| ByteDance | `Bytespider` | | |
| Common Crawl | `CCBot` | | |
| DuckDuckGo | | `DuckAssistBot` | |

Blocking a training bot never costs points in SagBot. Blocking a search or user-triggered bot does.

**SagBot reads** the rules in your `/robots.txt` for each AI search and user agent it knows.

## Point bots at a sitemap

A sitemap lists every page so crawlers don't have to discover them link by link. Declare it in
`robots.txt` so bots find it without guessing the path.

**SagBot reads** your sitemap (the one `robots.txt` declares, or `/sitemap.xml`) and how many URLs it lists.

## Publish an llms.txt

`/llms.txt` is a short Markdown file: your site's name, a one-paragraph summary, then links to the
pages that matter, each with a line of context. It gives a model the map a human would get from your
navigation. Serve it as text, not as an HTML page from your app router.

```markdown
# Acme

> Acme is invoicing software for freelancers: invoices, payment reminders and VAT returns in 14 EU countries.

## Product

- [Pricing](https://example.com/pricing): plans and what each one includes
- [API reference](https://example.com/docs/api): REST endpoints and authentication
- [Changelog](https://example.com/changelog): what shipped and when
```

SagBot's own: [sagbot.app/llms.txt](https://sagbot.app/llms.txt).

**SagBot reads** your `/llms.txt`: whether it exists and is text rather than an HTML page.

## Answer Accept: text/markdown with Markdown

Agents that fetch your pages pay per token. When a request says `Accept: text/markdown`, return the
same page as Markdown and add `Vary: Accept` so caches keep the two versions apart.

```bash
curl -s -o /dev/null -D - -H 'Accept: text/markdown' https://example.com/ \
  | grep -i -E '^(content-type|vary)'
# want: content-type: text/markdown; charset=utf-8
#       vary: Accept
```

**SagBot reads** your site's response to a request with `Accept: text/markdown`: its content type and `Vary` header.

## Send Link headers

A `Link` response header can point agents at your `llms.txt`, API catalog or Markdown version before
they parse a byte of HTML:

```http
Link: </llms.txt>; rel="describedby"; type="text/markdown"
```

**SagBot reads** the `Link` header on your site's response.

## If you run bots, publish a key directory

Web Bot Auth lets sites verify that a request really comes from your bot: you sign requests and
publish your public keys at `/.well-known/http-message-signatures-directory`. Skip it if you don't
operate a crawler or agent; SagBot shows it as advice, not a score.

**SagBot reads** `/.well-known/http-message-signatures-directory` on your site.

## See which of these your site passes

Run a free scan at **[sagbot.app](https://sagbot.app/)**. Each failing check comes with a fix
written for a coding agent: paste it into Claude Code, Cursor or Copilot.

---

**Also for your team:** [Marketing and PR](marketing.md) · [SEO teams](seo.md) · [Content teams](content.md) · [Online shops](shops.md) · [SaaS and API products](saas.md)
**Guides:** [Improve your GEO](../guides/improve-geo.md) · [AI visibility](../guides/ai-visibility.md) · [All 40 checks](../checks.md)
