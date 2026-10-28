# SagBot: free AI-readiness checker for websites

[![Scan your site](https://img.shields.io/badge/Scan%20your%20site-sagbot.app-111?style=for-the-badge)](https://sagbot.app/)

**[SagBot](https://sagbot.app/) is a free AI-readiness checker.** Enter a domain and it runs 40 checks,
then gives two scores out of 100: **Agent-ready** (can AI bots and agents reach and use the site) and
**Answer-ready** (will AI answer engines such as ChatGPT, Perplexity and Gemini cite it). Every failing
check comes with a fix written for coding agents: paste it into Claude Code, Cursor or Copilot.

No account. No install. About 20 seconds per scan.

[![SagBot: see your site the way AI bots and agents do](https://sagbot.app/og-image.png)](https://sagbot.app/)

**Run a scan:** [sagbot.app](https://sagbot.app/) ·
**Prefill a domain:** `https://sagbot.app/?domain=example.com` ·
**All 40 checks:** [checks.md](checks.md)

## Contents

- [What does SagBot check?](#what-does-sagbot-check)
- [How are the scores calculated?](#how-are-the-scores-calculated)
- [How does a scan work?](#how-does-a-scan-work)
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
| **SEO** | Answer-ready | The basics every answer engine still leans on: status code, HTTPS, indexability, canonical URLs, titles and descriptions. |
| **GEO** | Answer-ready | How quotable your page is for generative engines: a clear entity, valid JSON-LD, `sameAs` profiles, server-rendered text. |
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
3. It runs the checks, scores them and gives the report its own share link: `https://sagbot.app/r/{id}`.
4. Copy one fix, or all of them as a single prompt, into your coding agent and review the changes before you ship.

The latest report for any host lives at `https://sagbot.app/d/{host}`. Every SagBot page is also
served as Markdown: `curl -H 'Accept: text/markdown' https://sagbot.app/r/{id}`.

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

## FAQ

### What is SagBot?

SagBot is a free AI-readiness checker. Enter a domain and it runs 40 checks, then gives two scores
out of 100: Agent-ready and Answer-ready, with a copy-paste fix for every failing check.

### Why is it called SagBot?

Sag stands for SEO, GEO and AEO, the three parts of the Answer-ready score. Bot is the fetcher that
reads your site when someone runs a scan.

### What is the difference between SEO, GEO and AEO?

SEO (search engine optimization) gets pages indexed and ranked. GEO (generative engine optimization)
makes them easy for generative AI to quote. AEO (answer engine optimization) gives questions direct
answers that assistants can repeat as written.

### Is SagBot free? Do I need an account?

Yes, scans are free and need no account. Score tracking over time is on the way; you can sign up for
it on any report.

### Does blocking GPTBot remove my site from ChatGPT search?

No. GPTBot collects training data. ChatGPT search uses OAI-SearchBot, and ChatGPT-User fetches a page
when someone asks about it. The SagBot report shows which of these your `robots.txt` allows.

### Does SagBot check whether ChatGPT already cites my site?

No. It checks whether AI crawlers can reach your site and whether your pages are easy to quote.
Whether an engine cites you also depends on your site's authority and on what people ask.

### Will a scan slow my site down?

No. A scan makes a handful of GET and HEAD requests. Each request stops after 8 seconds or 2 MB, and
a finished result is reused for an hour.

### How do I apply the fixes?

Copy one fix, or all of them as one prompt, paste it into Claude Code, Cursor or Copilot in your
site's repo, and review the changes before you ship. Fixes are always in English.

## For site owners: the SagBot fetcher

SagBot visits a site only when someone asks for a scan of it. No crawling, no schedule.

```http
User-Agent: SagBot/1.0 (+https://sagbot.app/bot)
```

To opt out, add this to your `robots.txt`:

```text
User-agent: SagBot
Disallow: /
```

What it fetches, hard limits and how redirects work: **[bot.md](bot.md)**.

## Links

- Website: [sagbot.app](https://sagbot.app/)
- How SagBot works: [sagbot.app/bot](https://sagbot.app/bot)
- Who it's for: [sagbot.app/for](https://sagbot.app/for)
- `llms.txt`: [sagbot.app/llms.txt](https://sagbot.app/llms.txt)
- Privacy: [sagbot.app/privacy](https://sagbot.app/privacy)
- Contact: [bot@sagbot.app](mailto:bot@sagbot.app)

Available in English, Ukrainian ([/uk](https://sagbot.app/uk)), Spanish ([/es](https://sagbot.app/es)),
Portuguese ([/pt](https://sagbot.app/pt)), German ([/de](https://sagbot.app/de)),
French ([/fr](https://sagbot.app/fr)), Polish ([/pl](https://sagbot.app/pl)) and Italian ([/it](https://sagbot.app/it)).

Made in Ukraine.
