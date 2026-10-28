# How to improve your GEO: seven steps to get cited by AI answer engines

**Generative Engine Optimization (GEO) is making your site easy for AI answer engines to read, trust
and quote.** To improve it: let AI bots in, render text on the server, define your entity, make your
JSON-LD valid, write answers first, show author and date, and publish an `llms.txt`. Work through the
steps in order: each one only pays off once the step before it is in place.

[Check your GEO with SagBot](https://sagbot.app/) · [Full guide on sagbot.app](https://sagbot.app/guides/improve-geo) · [README](../README.md#guides)

## The seven steps

| Step | What to do | SagBot checks |
|---|---|---|
| 1 | Let AI bots in | `agent-robots`, `agent-ai-bots` |
| 2 | Put your text in the HTML | `geo-citable-text` |
| 3 | Say who you are | `geo-entity`, `geo-entity-name`, `geo-sameas` |
| 4 | Make your structured data valid | `geo-jsonld` |
| 5 | Write answers first | `aeo-question-headings`, `aeo-concise-answers` |
| 6 | Show author and date | `aeo-author-date` |
| 7 | Publish an llms.txt | `agent-llms-txt`, `aeo-llms-quality` |

## 1. Let AI bots in

Nothing else matters if the bot can't fetch your pages. Make sure `robots.txt` parses and doesn't
block the AI search and user agents (`OAI-SearchBot`, `PerplexityBot`, `ChatGPT-User`, `Claude-User`
and others). Blocking training bots is a separate choice.

Full list of the 24 AI bots SagBot checks: [for/developers.md](../for/developers.md#let-ai-search-and-assistant-bots-in).

**SagBot reads** your `robots.txt` and its rules for each AI bot it knows.

## 2. Put your text in the HTML

Most AI crawlers don't run JavaScript. Render your main text on the server, with real `h1` and `h2`
headings, so the crawler gets words instead of an empty app shell.

```bash
# What a crawler without JavaScript sees
curl -s https://example.com/ | sed 's/<[^>]*>/ /g' | wc -w
```

**SagBot reads** your site's text without JavaScript: word count and headings.

## 3. Say who you are

Add Organization (or Person) JSON-LD with your name, URL and logo; use the same name in your
`<title>` and `og:site_name`; and list your official profiles in `sameAs`. Together they let an
answer engine name you with confidence.

Example markup: [for/marketing.md](../for/marketing.md#link-your-official-profiles).

**SagBot reads** the Organization or Person data on your site, its name and its `sameAs` links.

## 4. Make your structured data valid

Every JSON-LD block should parse and have a `@type`. A single syntax error can make an engine ignore
the whole block.

**SagBot reads** every JSON-LD block on your site.

## 5. Write answers first

Phrase a few headings as the questions people ask, and put a short, self-contained answer (40 to 320
characters) right under each. That paragraph is what gets quoted.

```markdown
## How much does Acme cost?

Acme is free for up to five invoices a month. Unlimited invoices cost 9 euros a month.
```

**SagBot reads** your headings and the paragraph after each one.

## 6. Show author and date

Name who is behind the content and when it was updated, in the markup as well as on the page. Fresh,
attributed content wins ties.

**SagBot reads** author and date signals on your site.

## 7. Publish an llms.txt

Finish with `/llms.txt`: a short Markdown map of your most important pages, each with a line of
context. It's the cheapest way to point a model at the right pages.

Example file: [for/developers.md](../for/developers.md#publish-an-llmstxt).

**SagBot reads** your `/llms.txt`.

## See where your site stands

A free scan at **[sagbot.app](https://sagbot.app/)** checks every step above and gives you a fix for
each one that fails.

---

**Read next:** [AI visibility: what you can fix, and what you can only track](ai-visibility.md)
**By team:** [Developers](../for/developers.md) · [Marketing and PR](../for/marketing.md) · [SEO teams](../for/seo.md) · [Content teams](../for/content.md) · [Online shops](../for/shops.md) · [SaaS and API products](../for/saas.md)
