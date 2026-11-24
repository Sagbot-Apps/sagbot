# AI readiness for agencies: what to fix first on a client's site

**What should you fix first on a client's site?** A new client asks why AI assistants never mention
them. These six checks make a good first pass: each one is easy to explain to a client, a scan shows
whether it passes, and the next report shows what changed.

[Scan a client's site with SagBot](https://sagbot.app/) · [Full page on sagbot.app](https://sagbot.app/for/agencies) · [README](../README.md#who-is-sagbot-for)

## Checklist

| Do this | Where | SagBot check |
|---|---|---|
| Let AI search and user bots in | `/robots.txt` | `agent-ai-bots` |
| Add an llms.txt | `/llms.txt` | `agent-llms-txt` |
| Use one brand name everywhere | `<title>`, `og:site_name`, structured data | `geo-entity-name` |
| Fix JSON-LD that doesn't parse | every JSON-LD block | `geo-jsonld` |
| Make sure the text is in the HTML | server-rendered HTML | `geo-citable-text` |
| Put the answer right under the heading | the paragraph after each heading | `aeo-concise-answers` |

## Check that AI search bots can get in

A client's `robots.txt` may not have been read in years. Blocking `GPTBot` keeps content out of
training, which is a policy choice. Blocking `OAI-SearchBot`, `PerplexityBot` or `ChatGPT-User`
keeps the site out of their answers, and it's rarely what the client meant.

**SagBot reads** the rules in `/robots.txt` for each AI search and user agent it knows.

## Add an llms.txt

`/llms.txt` is a short Markdown file: the site's name, a one-paragraph summary, then links to the
pages that matter, each with a line of context. It makes an easy first deliverable: you can write it
from the client's navigation, and the next scan shows it in place.

**SagBot reads** `/llms.txt`: whether it exists and is text rather than an HTML page.

## Use one brand name everywhere

An answer engine has to be sure that "Acme", "Acme Inc." and "acme.io" are one company before it
credits the client. Agree on one name with them and use it in the page title, the social preview and
the structured data.

**SagBot reads** the name in the site's structured data and checks that it matches the `<title>` and
`og:site_name`.

## Fix JSON-LD that doesn't parse

A site that has been through several themes and plugins often carries structured data from each of
them. One broken comma can void a whole block, so validate every block and drop the ones the client
no longer needs.

**SagBot reads** every JSON-LD block on the site: whether it parses and whether its nodes have a
`@type`.

## Make sure the text is in the HTML

Many AI crawlers don't run JavaScript. If the client's words only appear after a script runs, those
crawlers see an empty shell. Rendering the text on the server fixes it, and "view source" lets the
client see the difference.

**SagBot reads** the text on the site without running JavaScript: how many words, and whether it has
headings.

## Put the answer right under the heading

The passage answer engines lift is usually the paragraph straight after a heading. Rewrite it as the
answer: one to three sentences, 40 to 320 characters, that still make sense on their own. Side by
side with the old one, it's a change a client can see for themselves.

**SagBot reads** the paragraph after each heading on the site and checks its length.

## Show clients what got fixed

- **[Agency reports](https://sagbot.app/agency-reports):** reports on any client site under your
  brand, with scores before and after and the checks that were fixed or broke. $1.99 a report, once
  or on a schedule.
- **[Monitoring](https://sagbot.app/monitoring):** for client domains you can verify with a DNS
  record, rescans on a schedule and an email when a score changes.
- **[MCP & API](https://sagbot.app/mcp):** scan client sites and create their reports from Claude or
  Cursor through SagBot's MCP server.

## Scan a client's site

Run a free scan at **[sagbot.app](https://sagbot.app/)** before the kickoff call. Each failing check
comes with a fix to paste into a coding agent or send to the client's developer.

---

**Also for your team:** [Developers](developers.md) · [Marketing and PR](marketing.md) · [SEO teams](seo.md) · [Content teams](content.md) · [Online shops](shops.md) · [SaaS and API products](saas.md)
**Guides:** [Improve your GEO](../guides/improve-geo.md) · [AI visibility](../guides/ai-visibility.md) · [All 44 checks](../checks.md)
