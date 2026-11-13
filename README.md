# SagBot: free AI-readiness checker for websites

[![Scan your site](https://img.shields.io/badge/Scan%20your%20site-sagbot.app-111?style=for-the-badge)](https://sagbot.app/)
[![SagBot score for sagbot.app](https://sagbot.app/badge/sagbot.app.svg)](https://sagbot.app/d/sagbot.app)

**[SagBot](https://sagbot.app/) is a free AI-readiness checker.** Enter a domain and it runs 44 checks,
then gives two scores out of 100: **Agent-ready** (can AI bots and agents reach and use the site) and
**Answer-ready** (will AI answer engines such as ChatGPT, Perplexity and Gemini cite it). Every failing
check comes with a fix written for coding agents: paste it into Claude Code, Cursor or Copilot.

No install. About 20 seconds per scan. 3 free scans a day without an account; a free account gives you more.

[![SagBot: see your site the way AI bots and agents do](https://sagbot.app/og-image.png)](https://sagbot.app/)

**Run a scan:** [sagbot.app](https://sagbot.app/) ·
**Scan from a link:** `https://sagbot.app/?domain=example.com` ·
**All 44 checks:** [checks.md](checks.md)

## Contents

- [What does SagBot check?](#what-does-sagbot-check)
- [How are the scores calculated?](#how-are-the-scores-calculated)
- [How does a scan work?](#how-does-a-scan-work)
- [Add a score badge to your site](#add-a-score-badge-to-your-site)
- [Scan from your coding agent (MCP)](#scan-from-your-coding-agent-mcp)
- [AI visibility API](#ai-visibility-api)
- [Free tools](#free-tools)
- [Who is SagBot for?](#who-is-sagbot-for)
- [Guides](#guides)
- [FAQ](#faq)
- [For site owners: the SagBot fetcher](#for-site-owners-the-sagbot-fetcher)
- [Links](#links)

## What does SagBot check?

SagBot checks the files, headers and markup that decide whether AI can read your site and quote it.
The checks fall into four groups:

| Group | Score | What it looks at |
|---|---|---|
| **Agent** | Agent-ready | Whether AI crawlers, search bots and browsing agents are allowed in, and whether they find Markdown, `llms.txt` and `/.well-known` discovery files once they arrive. |
| **SEO** | Answer-ready | The basics every answer engine still leans on: status code, HTTPS, indexability, Googlebot access, snippet controls, crawlable links, canonical URLs, titles and descriptions. |
| **GEO** | Answer-ready | How quotable your page is for generative engines: a clear entity, valid JSON-LD that matches the page, `sameAs` profiles, server-rendered text. |
| **AEO** | Answer-ready | Whether questions on the page have direct answers and the schema to match, so assistants can lift them as written. |

What a single scan covers:

- `robots.txt` rules for 24 AI bots from OpenAI, Anthropic, Google, Perplexity, Apple, Meta, Amazon, Mistral, ByteDance, DuckDuckGo and Common Crawl
- `Content-Signal` (search, AI answers, AI training)
- XML sitemap, `llms.txt` and `llms-full.txt`
- Markdown for agents (`Accept: text/markdown`) and `Link` headers
- JSON-LD, Organization / Person entity, `sameAs`, OpenGraph, meta description, title, canonical, `hreflang`
- Concise answer paragraphs, question headings, FAQPage / QAPage schema, author and date
- Agent protocols for APIs and apps: RFC 9727 API catalog, OAuth / OIDC discovery, RFC 9728 protected resource metadata, MCP server card, A2A agent card, agent skills index, `ai-catalog`, `auth.md`, WebMCP
- Agentic commerce for shops: Universal Commerce Protocol (UCP), Agentic Commerce Protocol (ACP), x402, MPP

Pick a site type before scanning (**Content site**, **API or app**, **Shop**) and SagBot runs the
checks that apply to it. Full list with weights: **[checks.md](checks.md)**.

## How are the scores calculated?

Each check returns pass, partial or fail. A pass earns its full weight, a partial earns half, a
fail earns nothing.

- **Agent-ready** = weighted share of passed Agent checks, 0–100.
- **Answer-ready** = average of the SEO, GEO and AEO scores, 0–100.
- Checks marked *advice* are shown with a fix but never change a score (new or optional specs such as WebMCP, `auth.md`, x402).
- Checks SagBot couldn't see (a timeout, a blocked request) count as *couldn't check*, never as a fail.
- Fixes are ranked by how many points each one adds, biggest first.

## How does a scan work?

1. Enter a domain on [sagbot.app](https://sagbot.app/) and pick a site type.
2. SagBot reads `robots.txt`, the homepage (as HTML and as Markdown), the sitemap, `llms.txt` and a few `/.well-known` files.
3. It runs the checks, scores them and shows the report on the domain's page: `https://sagbot.app/d/{host}`.
4. Copy one fix, or all of them as a single prompt, into your coding agent and review the changes before you ship.

Each domain has one page, and it always shows the latest result, so that's the link to share. Older
`/r/{id}` links redirect to it. Every SagBot page is also served as Markdown:
`curl -H 'Accept: text/markdown' https://sagbot.app/d/{host}`.

## Add a score badge to your site

Show your scores on your site or GitHub profile. The badge is live: it updates within an hour of a new
scan and links to your report.

| Look | Example | Image URL |
|---|---|---|
| Full | [![SagBot score for sagbot.app](https://sagbot.app/badge/sagbot.app.svg)](https://sagbot.app/d/sagbot.app) | `https://sagbot.app/badge/{host}.svg` |
| Compact | [![SagBot score for sagbot.app](https://sagbot.app/badge/sagbot.app.svg?style=compact)](https://sagbot.app/d/sagbot.app) | `https://sagbot.app/badge/{host}.svg?style=compact` |

Markdown:

```markdown
[![SagBot score for example.com](https://sagbot.app/badge/example.com.svg)](https://sagbot.app/d/example.com)
```

HTML:

```html
<a href="https://sagbot.app/d/example.com" target="_blank" rel="noopener">
  <img src="https://sagbot.app/badge/example.com.svg" alt="SagBot score for example.com" />
</a>
```

Every report has a generator that fills in your domain. `Agent` on the badge is the Agent-ready score
and `SEO·GEO·AEO` is the Answer-ready score. A domain without a result shows `not scanned`.

## Scan from your coding agent (MCP)

With a free account, your coding agent can scan your site and fix what it finds without leaving your repo.
SagBot's MCP server is `https://mcp.sagbot.app/mcp` (Streamable HTTP). Create a personal API token in
[Account → Credentials](https://sagbot.app/account/credentials) (it's shown once), then add the server.
Step-by-step setup for each client, with your token filled in: [sagbot.app/mcp](https://sagbot.app/mcp).

Claude Code, for every project:

```bash
claude mcp add --transport http --scope user sagbot https://mcp.sagbot.app/mcp \
  --header "Authorization: Bearer sgb_…"
```

Cursor, Gemini CLI and Windsurf take the same JSON entry; only the URL key differs:

```json
{ "mcpServers": { "sagbot": { "url": "https://mcp.sagbot.app/mcp", "headers": { "Authorization": "Bearer sgb_…" } } } }
```

| Client | File | URL key |
|---|---|---|
| Cursor | `~/.cursor/mcp.json` | `url` |
| Gemini CLI | `~/.gemini/settings.json` | `httpUrl` |
| Windsurf | `~/.config/devin/mcp_config.json` (older installs: `~/.codeium/windsurf/mcp_config.json`) | `serverUrl` |

OpenAI Codex CLI reads the token from an environment variable (`export SAGBOT_TOKEN="sgb_…"` in your shell
profile, or a file it sources) and the server from `~/.codex/config.toml`:

```toml
[mcp_servers.sagbot]
url = "https://mcp.sagbot.app/mcp"
bearer_token_env_var = "SAGBOT_TOKEN"
```

VS Code (Copilot agent mode) asks for the token instead of storing it, in `.vscode/mcp.json` or your user MCP
configuration:

```json
{
  "inputs": [{ "type": "promptString", "id": "sagbot-token", "description": "SagBot token", "password": true }],
  "servers": {
    "sagbot": { "type": "http", "url": "https://mcp.sagbot.app/mcp", "headers": { "Authorization": "Bearer ${input:sagbot-token}" } }
  }
}
```

Keep the token out of git: don't add it with `claude mcp add --scope project` (that writes `.mcp.json`) or in a
committed config file. Only bearer tokens work; sign-in from the MCP client (OAuth) isn't supported yet.

| Tool | What it does |
|---|---|
| `scan_site` | Scans a domain (`siteType`: `content`, `app` or `shop`; `rescan: true` for a fresh scan) and returns both scores plus every failed or partial check with its fix. |
| `get_latest_scan` | Returns the newest stored scan of a domain without scanning. |

SagBot scans the public site, so deploy your fixes (or use a public preview URL) before you rescan.
Limits: 3 scans a minute per account; a rescan of the same site within 2 minutes returns the stored
result. MCP scans show up in your dashboard marked "via MCP". Revoke a token any time from the same page.

## AI visibility API

Ask AI answer engines a question and see whether your brand is mentioned and cited, at what position, next to
which competitors, and which URLs they cite. Base URL: `https://api.sagbot.app`.

**Auth.** Create a token in [Account → Credentials](https://sagbot.app/account/credentials) and send it as
`Authorization: Bearer sgb_…`. `GET /v1/engines` and `GET /v1/health` need no token.

**Pricing.** You pay per answer (one engine, one sample) from your prepaid USD balance on sagbot.app.
Top up at [Account → Billing](https://sagbot.app/account/billing) ($25, $50 or $100). Failed engine runs are
returned to your balance automatically. When a check or scheduled scan takes your balance below $2.00, we email
you once; topping up re-arms that email.

| Engine id | Engine | Price per answer |
|---|---|---|
| `chatgpt` | ChatGPT | $0.035 |
| `google_aio` | Google AI Overviews | $0.035 |
| `gemini` | Gemini | $0.035 |
| `google_ai_mode` | Google AI Mode | $0.035 |
| `perplexity` | Perplexity | $0.035 |
| `claude` | Claude (API, web search on) | $0.06 |
| `chatgpt_api` | ChatGPT (API, web search on) | $0.06 |
| `gemini_api` | Gemini (API, web search on) | $0.06 |

A check costs the sum of `price × samples` over its engines. Without `engines`, a check runs the five
$0.035 engines. `GET /v1/engines` is the live catalog (prices, status, supported options).

### Create a check

`POST /v1/check` takes `query` (≤ 300 characters), `brand` (`name`, optional `domain` and `aliases`), and
optionally `competitors`, `engines`, `country` (ISO code, default `US`), `language` (default `en`) and
`samples` (1 to 5, default 1; more samples give a mention rate with a 95% interval). Add `"dry_run": true` to get
the price without paying. Send an `Idempotency-Key` header (≤ 64 characters) to retry safely: a repeat returns the
same check and never charges twice.

```sh
curl https://api.sagbot.app/v1/check \
  -H "Authorization: Bearer $SGB" -H "content-type: application/json" \
  -d '{
    "query": "best tool to check if my website is ready for ai agents",
    "brand": { "name": "SagBot", "domain": "sagbot.app" },
    "competitors": ["Acme Audit"],
    "engines": ["chatgpt", "perplexity"],
    "samples": 3
  }'
```

```json
{ "id": "chk_0f8e6c1d2b3a49e8a7c6b5d4e3f2a1b0", "status": "queued", "cost_usd": 0.21 }
```

The response headers `X-Cost-USD` and `X-Balance-USD` show the price and your balance after paying.

### Get a check

Poll `GET /v1/checks/{id}` until `status` is `done`, `partial` (some answers failed and were refunded) or
`failed`. It is `queued` or `running` before that. Only the token owner can read a check.

```sh
curl https://api.sagbot.app/v1/checks/chk_0f8e6c1d2b3a49e8a7c6b5d4e3f2a1b0 -H "Authorization: Bearer $SGB"
```

Each entry in `results` has `mentioned_rate` with its 95% interval `ci95`, `cited_rate`, `avg_position`
(1 = mentioned first), per-competitor rates, the `citations` (URLs, most cited first) and, for API engines,
the web searches the model ran (`fan_out`). Shortened:

```json
{
  "id": "chk_0f8e6c1d2b3a49e8a7c6b5d4e3f2a1b0",
  "status": "done",
  "cost_usd": 0.21,
  "refunded_usd": 0,
  "results": [
    {
      "engine": "chatgpt",
      "samples": 3,
      "samples_ok": 3,
      "mentioned_rate": 0.67,
      "ci95": [0.21, 0.94],
      "cited_rate": 0.33,
      "avg_position": 1.5,
      "competitors": [{ "name": "Acme Audit", "mentioned_rate": 0.33, "avg_position": 2 }],
      "citations": [{ "url": "https://sagbot.app/", "domain": "sagbot.app", "count": 1, "brand": true }]
    }
  ]
}
```

### List engines and health

```sh
curl https://api.sagbot.app/v1/engines   # id, label, price_usd, default, status, supported options
curl https://api.sagbot.app/v1/health    # success rate and p95 latency per engine, last 24 hours
```

`GET /v1/usage?days=30` (token required) returns what you spent per day and engine; refunded answers are not counted.

### Errors and limits

Errors look like `{ "error": { "code": "…", "message": "…" } }`.

| Status | `code` | Meaning |
|---|---|---|
| 400 | `invalid_request` | Bad body or parameter; the message says which |
| 401 | `unauthorized` | Missing or invalid token |
| 402 | `insufficient_balance` | The check costs more than your balance; top up and retry with a new `Idempotency-Key` |
| 404 | `not_found` | Unknown check id, or a check that is not yours |
| 413 | `body_too_large` | Request body too large |
| 429 | `rate_limited` | Too many requests; wait a minute |

Limits: 10 paid checks a minute per token, and 60 authenticated requests a minute per IP address.

## Free tools

Need one answer instead of a full scan? Each [free tool](https://sagbot.app/tools) checks one thing about a domain.

| Tool | What it answers |
|---|---|
| [llms.txt checker](https://sagbot.app/tools/llms-txt-checker) | Does this site serve a well-formed `llms.txt` file? |
| [llms.txt generator](https://sagbot.app/tools/llms-txt-generator) | Draft an `llms.txt` from up to 50 of this site's pages, edit it, then copy or download it. |
| [AI bots in robots.txt](https://sagbot.app/tools/ai-bots-robots-checker) | What does this site's `robots.txt` tell AI bots? |
| [robots.txt validator](https://sagbot.app/tools/robots-txt-validator) | Is this `robots.txt` valid, line by line, and which search, AI and SEO crawlers may reach a given URL? |
| [AI crawler log analyzer](https://sagbot.app/tools/ai-crawler-log-analyzer) | Which AI crawlers visit my site, are they genuine, and which pages fail for them? Reads your access log in the browser; nothing is uploaded. |
| [Schema checker](https://sagbot.app/tools/schema-checker) | Is the JSON-LD on this homepage valid? |
| [JSON-LD visualizer](https://sagbot.app/tools/json-ld-visualizer) | Which entities does this page's JSON-LD describe, how do they link, and roughly how may search show them? |
| [Open Page Rank checker](https://sagbot.app/tools/open-page-rank-checker) | What is this domain's Open Page Rank score? |
| [SSL checker](https://sagbot.app/tools/ssl-checker) | What SSL/TLS grade does this site get? |
| [AI agent checkout checker](https://sagbot.app/tools/agent-checkout-checker) | Can AI shopping agents reach this store's checkout, according to a saved check? |
| [Whois lookup](https://sagbot.app/tools/whois-lookup) | Who registered this domain, and when does it expire? |
| [Security headers checker](https://sagbot.app/tools/security-headers-checker) | What HTTP security grade does this site get? |
| [Wayback Machine checker](https://sagbot.app/tools/wayback-checker) | When was this domain first and last archived? |
| [Wikipedia and Wikidata checker](https://sagbot.app/tools/wikipedia-wikidata-checker) | Does Wikidata list this domain as an official website? |
| [Open Graph preview](https://sagbot.app/tools/open-graph-preview) | What card does this page show when someone shares its link, and which tags are missing? |

## Who is SagBot for?

AI readiness isn't one team's job. Each check lands with whoever owns that part of the site.

| Team | Question SagBot answers | Read |
|---|---|---|
| Developers | Can AI crawlers and agents reach and read my site? | [for/developers.md](for/developers.md) |
| Marketing and PR | Will AI answers recognise and name my brand? | [for/marketing.md](for/marketing.md) |
| SEO teams | Which SEO basics do AI answers still depend on? | [for/seo.md](for/seo.md) |
| Content teams | How do I write pages that AI answers quote? | [for/content.md](for/content.md) |
| Online shops | Can AI shopping agents find my products and check out? | [for/shops.md](for/shops.md) |
| SaaS and API products | Can AI agents discover and connect to my product? | [for/saas.md](for/saas.md) |

## Guides

- **[Improve your GEO in seven steps](guides/improve-geo.md)**: how to get your site cited by AI answer engines.
- **[AI visibility: what you can fix, and what you can only track](guides/ai-visibility.md)**: where to spend effort first.
- **[See which AI crawlers really visit your site](guides/ai-crawler-logs.md)**: find your access log, count AI crawler visits, tell real crawlers from fakes.

## FAQ

### What is SagBot?

SagBot is a free AI-readiness checker. Enter a domain and it runs 44 checks, then gives two scores
out of 100: Agent-ready and Answer-ready, with a copy-paste fix for every failing check.

### Why is it called SagBot?

Sag stands for SEO, GEO and AEO, the three parts of the Answer-ready score. Bot is the fetcher that
reads your site when someone runs a scan.

### What is the difference between SEO, GEO and AEO?

SEO (search engine optimization) gets pages indexed and ranked. GEO (generative engine optimization)
makes them easy for generative AI to quote. AEO (answer engine optimization) gives questions direct
answers that assistants can repeat as written.

### Is SagBot free? Do I need an account?

Yes, it's free. Without an account you get 3 new scans a day. A free account adds rescans any time and
keeps your scan history, with the change in both scores since your last scan of each domain. Sign in
with an email code, a passkey, Google or GitHub: there are no passwords. Weekly checks with email
alerts are on the way. Free tools stay free with no account and no daily limit. Paid plans are coming too: join the waitlist on [sagbot.app/pricing](https://sagbot.app/pricing).

### Can I hide my site's result?

Yes, if you own the site. Sign in, add the `sagbot-verification=…` TXT record SagBot gives you to your
domain's DNS, and hide the site from your dashboard. Visitors then see "The result is hidden by the
owner": no scores on the page, its Markdown version or the share card, the badge shows `hidden`, and
the page leaves Google. Signed-in users still see the full result. Make it public again at any time.

### Does blocking GPTBot remove my site from ChatGPT search?

No. GPTBot collects training data. ChatGPT search uses OAI-SearchBot, and ChatGPT-User fetches a page
when someone asks about it. The SagBot report shows which of these your `robots.txt` allows.

### Does SagBot check whether ChatGPT already cites my site?

No. It checks whether AI crawlers can reach your site and whether your pages are easy to quote.
Whether an engine cites you also depends on your site's authority and on what people ask.

### Will a scan slow my site down?

No. A scan makes a handful of GET and HEAD requests. Each request stops after 8 seconds or 2 MB, and
a finished result is reused for an hour.

### Can a SagBot report appear in Google?

Yes. Reports are public, and the English page of a domain with two or more complete scans can be
indexed. To remove it, add the opt-out rule below to your `robots.txt`, then scan your site once. SagBot
doesn't crawl, so the rule takes effect at that scan.

### How do I apply the fixes?

Copy one fix, or all of them as one prompt, paste it into Claude Code, Cursor or Copilot in your
site's repo, and review the changes before you ship. Fixes are always in English. With a free account,
your coding agent can also scan and fetch the fixes itself: see [Scan from your coding agent (MCP)](#scan-from-your-coding-agent-mcp).

## For site owners: the SagBot fetcher

SagBot visits a site only when someone runs a scan or a free tool on it. No crawling, no schedule.

```http
User-Agent: SagBot/1.0 (+https://sagbot.app/bot)
```

To opt out, add this to your `robots.txt`:

```text
User-agent: SagBot
Disallow: /
```

The opt-out takes effect the next time someone scans your site, so run one scan after adding it: the
report then leaves Google and your badge shows `not scanned`.

A site-wide `User-agent: *` / `Disallow: /` keeps SagBot out too. Requests are signed with Web Bot Auth;
the public key is at `https://sagbot.app/.well-known/http-message-signatures-directory`.

What it fetches, hard limits, how redirects work and how free tools behave: **[bot.md](bot.md)**.

## Links

- Website: [sagbot.app](https://sagbot.app/)
- How SagBot works: [sagbot.app/bot](https://sagbot.app/bot)
- Who it's for: [sagbot.app/for](https://sagbot.app/for)
- Free tools: [sagbot.app/tools](https://sagbot.app/tools)
- Pricing (paid plans coming soon, free scans stay free): [sagbot.app/pricing](https://sagbot.app/pricing)
- `llms.txt`: [sagbot.app/llms.txt](https://sagbot.app/llms.txt)
- Privacy: [sagbot.app/privacy](https://sagbot.app/privacy)
- Contact: [bot@sagbot.app](mailto:bot@sagbot.app)

Available in English, Ukrainian ([/uk](https://sagbot.app/uk)), Spanish ([/es](https://sagbot.app/es)),
Portuguese ([/pt](https://sagbot.app/pt)), German ([/de](https://sagbot.app/de)),
French ([/fr](https://sagbot.app/fr)), Polish ([/pl](https://sagbot.app/pl)) and Italian ([/it](https://sagbot.app/it)).

Made in Ukraine.
