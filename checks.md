# All 40 SagBot checks: AI readiness, SEO, GEO and AEO

SagBot runs 40 checks on a site and groups them into four buckets. **Agent** checks make up the
Agent-ready score. **SEO**, **GEO** and **AEO** checks make up the Answer-ready score (the average of
the three). Each check below lists its weight in its bucket and the site types it runs for, and links to the spec or documentation it follows.

[Scan your site on sagbot.app](https://sagbot.app/) · [Back to README](README.md)

**How to read the tables**

- **Weight**: a pass earns the full weight, a partial earns half, a fail earns nothing.
- **Advice**: shown with a fix, never scored. Used for specs that are new, optional or can't be verified from outside.
- **Site types**: *All*, *API or app*, or *Shop*. A check outside its site type still runs if your site publishes that file anyway; otherwise it shows as not applicable.

## Agent checks (Agent-ready score)

Can AI crawlers, search bots and agents reach the site and find machine-readable versions of it?

### Discoverability

| Check | ID | Weight | Site types | Why it matters |
|---|---|---|---|---|
| [robots.txt](https://www.rfc-editor.org/rfc/rfc9309) | `agent-robots` | 3 | All | robots.txt is the first file every crawler and AI agent reads to learn what it may fetch. |
| [XML sitemap](https://www.sitemaps.org/protocol.html) | `agent-sitemap` | 2 | All | A sitemap lists every page so crawlers and agents don't have to discover them by following links. |
| [Link headers](https://www.rfc-editor.org/rfc/rfc8288) | `agent-link-headers` | 2 | All | Link response headers point agents at machine-readable versions of your site without parsing HTML. |

### Content

| Check | ID | Weight | Site types | Why it matters |
|---|---|---|---|---|
| [Markdown for agents](https://developers.cloudflare.com/fundamentals/reference/markdown-for-agents/) | `agent-markdown` | 3 | All | Serving Markdown on `Accept: text/markdown` gives agents clean text at a fraction of the tokens. |
| [llms.txt](https://llmstxt.org/) | `agent-llms-txt` | 3 | All | llms.txt gives language models a curated, plain-text map of your site. |

### Bot access

| Check | ID | Weight | Site types | Why it matters |
|---|---|---|---|---|
| [AI search & user agents allowed](https://www.rfc-editor.org/rfc/rfc9309) | `agent-ai-bots` | 3 | All | If AI search and assistant bots are blocked, your site can't be quoted or linked in AI answers. |
| [Content Signals](https://contentsignals.org/) | `agent-content-signals` | 1 | All | Content-Signal says how AI may use your content: search, answers, or training. |
| [Web Bot Auth](https://datatracker.ietf.org/wg/webbotauth/about/) | `agent-web-bot-auth` | Advice | All | Only matters if you run bots: a key directory lets sites verify your signed requests. |

### Protocol discovery

| Check | ID | Weight | Site types | Why it matters |
|---|---|---|---|---|
| [API catalog](https://www.rfc-editor.org/rfc/rfc9727) | `agent-api-catalog` | 2 | API or app | An RFC 9727 API catalog lets agents find your APIs and their OpenAPI descriptions in one request. |
| [OAuth / OIDC discovery](https://www.rfc-editor.org/rfc/rfc8414) | `agent-oauth` | 2 | API or app | Authorization server metadata lets agents sign in to your API without hard-coded endpoints. |
| [OAuth protected resource metadata](https://www.rfc-editor.org/rfc/rfc9728) | `agent-oauth-resource` | 1 | API or app | RFC 9728 metadata tells an agent which authorization server issues tokens for your API. |
| [MCP server card](https://github.com/modelcontextprotocol/experimental-ext-server-card) | `agent-mcp-card` | 2 | API or app | A server card lets MCP clients find and configure your MCP server from just your domain. |
| [A2A agent card](https://a2a-protocol.org/latest/specification/) | `agent-a2a-card` | 2 | API or app | An agent card lets other agents discover what your agent does and how to talk to it. |
| [Agent skills index](https://github.com/cloudflare/agent-skills-discovery-rfc) | `agent-skills-index` | 1 | API or app | A skills index hands agents ready-made instructions for using your site. |
| [WebMCP tools](https://webmachinelearning.github.io/webmcp/) | `agent-webmcp` | Advice | API or app | WebMCP lets in-browser agents call your site's actions as tools instead of clicking around. |
| [auth.md](https://github.com/workos/auth.md) | `agent-auth-md` | Advice | API or app | auth.md tells agents how to register and get credentials for your service. |
| [Agentic resource discovery (ai-catalog)](https://agenticresourcediscovery.org/) | `agent-ard` | Advice | API or app | An ai-catalog lists every agent-facing resource (MCP servers, agents, skills) in one manifest. |
| [Personal Agent Protocol](https://personalagentprotocol.org/docs/spec) | `agent-pap` | Advice | API or app | Discovery that tells personal agents which organisation runs the site and what it supports. |

### Commerce

| Check | ID | Weight | Site types | Why it matters |
|---|---|---|---|---|
| [Universal Commerce Protocol](https://ucp.dev/specification/overview/) | `agent-ucp` | 2 | Shop | A UCP profile lets shopping agents discover your checkout and payment options. |
| [x402 payments](https://x402.org) | `agent-x402` | Advice | Shop | x402 lets agents pay for a resource over plain HTTP with a 402 response. |
| [MPP payment discovery](https://mpp.dev) | `agent-mpp` | Advice | Shop | MPP marks payable API operations in your OpenAPI file so agents know what costs what. |
| [Agentic Commerce Protocol](https://agenticcommerce.dev) | `agent-acp` | Advice | Shop | An ACP discovery document lets agents find your agentic checkout API. |

## SEO checks (Answer-ready score)

The classic search basics that AI answers are still built on.

| Check | ID | Weight | Category | Why it matters |
|---|---|---|---|---|
| [Homepage status code](https://developers.google.com/search/docs/crawling-indexing/http-network-errors) | `seo-status` | 3 | Indexing | Crawlers only index pages that answer 200 OK. |
| [Indexable homepage](https://developers.google.com/search/docs/crawling-indexing/block-indexing) | `seo-indexability` | 3 | Indexing | A noindex keeps the page out of search, and out of AI answers built on search. |
| [Canonical URL](https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls) | `seo-canonical` | 2 | Indexing | A canonical link tells crawlers which URL is the real one, so signals aren't split. |
| [HTTPS](https://developers.google.com/search/docs/appearance/page-experience) | `seo-https` | 1 | Indexing | Search engines and AI agents prefer, and some require, secure pages. |
| [hreflang sanity](https://developers.google.com/search/docs/specialty/international/localized-versions) | `seo-hreflang` | 1 | Indexing | Broken hreflang can show the wrong language version in search and AI answers. |
| [Page title](https://developers.google.com/search/docs/appearance/title-link) | `seo-title` | 3 | Snippet | The `<title>` is the headline search engines and AI answers show for your site. |
| [Meta description](https://developers.google.com/search/docs/appearance/snippet) | `seo-meta-description` | 2 | Snippet | A good description is often used as the snippet under your result. |
| [OpenGraph tags](https://ogp.me/) | `seo-opengraph` | 2 | Snippet | OpenGraph tags control how links to your site look when shared or previewed. |

All SEO checks run for every site type.

## GEO checks (Answer-ready score)

Generative engine optimization: can a model tell who you are and quote you?

| Check | ID | Weight | Category | Why it matters |
|---|---|---|---|---|
| [JSON-LD structured data](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data) | `geo-jsonld` | 3 | Structured data | JSON-LD gives AI and search engines machine-readable facts about your site. |
| [Entity type](https://developers.google.com/search/docs/appearance/structured-data/organization) | `geo-entity` | 3 | Entity | An Organization or Person node tells AI who is behind the site, so it can cite you by name. |
| [sameAs profile links](https://schema.org/sameAs) | `geo-sameas` | 2 | Entity | sameAs links tie your entity to profiles AI already knows, so it's not confused with others. |
| [Entity name consistency](https://developers.google.com/search/docs/appearance/site-names) | `geo-entity-name` | 1 | Entity | Using the same name in title, og:site_name and schema helps AI recognise one entity. |
| [Citable server-rendered text](https://developers.google.com/search/docs/crawling-indexing/javascript/javascript-seo-basics) | `geo-citable-text` | 3 | Citability | Most AI crawlers don't run JavaScript: they can only cite text in the HTML. |

All GEO checks run for every site type.

## AEO checks (Answer-ready score)

Answer engine optimization: does the page hand assistants an answer they can repeat as written?

| Check | ID | Weight | Category | Why it matters |
|---|---|---|---|---|
| [FAQ / QAPage schema](https://schema.org/FAQPage) | `aeo-faq-schema` | 2 | Answers | FAQPage markup hands answer engines ready-made question/answer pairs. |
| [Concise answer paragraphs](https://developers.google.com/search/docs/appearance/featured-snippets) | `aeo-concise-answers` | 1 | Answers | A short paragraph right under a heading is the passage answer engines quote. |
| [llms.txt quality](https://llmstxt.org/) | `aeo-llms-quality` | 1 | Answers | A well-formed llms.txt gives AI a curated, quotable map of your site. |
| [Question-style headings](https://developers.google.com/search/docs/fundamentals/creating-helpful-content) | `aeo-question-headings` | Advice | Answers | Headings phrased as questions match how people ask AI assistants. |
| [Author and date signals](https://developers.google.com/search/docs/appearance/structured-data/article) | `aeo-author-date` | 1 | Trust | Answer engines favour content with a clear author and a visible date. |

All AEO checks run for every site type. FAQ schema is not applicable on a page without question headings.

## By team

Each team guide explains its checks with examples:
[developers](for/developers.md) ·
[marketing and PR](for/marketing.md) ·
[SEO](for/seo.md) ·
[content](for/content.md) ·
[online shops](for/shops.md) ·
[SaaS and API products](for/saas.md)

---

See how your site scores on all 40: **[sagbot.app](https://sagbot.app/)**.
