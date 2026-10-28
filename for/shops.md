# AI shopping agents and your online shop: UCP, ACP, x402 and MPP

**Can AI shopping agents find your products and check out?** Assistants are starting to shop on
people's behalf: compare products, fill the cart, pay. They do it through a handful of new protocols
that sit next to your storefront: Universal Commerce Protocol (UCP), Agentic Commerce Protocol (ACP),
x402 and MPP. SagBot shows which ones your shop already speaks, plus the basics agents read first.

[Check your shop with SagBot](https://sagbot.app/) · [Full page on sagbot.app](https://sagbot.app/for/shops) · [README](../README.md#who-is-sagbot-for)

## Checklist

| Do this | Where | SagBot check |
|---|---|---|
| Publish a Universal Commerce Protocol profile | `/.well-known/ucp` | `agent-ucp` |
| Expose an Agentic Commerce Protocol endpoint | `/.well-known/acp.json` | `agent-acp` (advice) |
| Let agents pay over HTTP with x402 | `402` + `Payment-Required` | `agent-x402` (advice) |
| Mark paid operations for MPP | `x-payment-info` in `/openapi.json` | `agent-mpp` (advice) |
| Describe your shop in structured data | Organization and Product JSON-LD | `geo-jsonld` |
| Serve Markdown to agents | `Accept: text/markdown` | `agent-markdown` |

Pick **Shop** as the site type when you scan to include the commerce checks.

## Publish a Universal Commerce Protocol profile

A UCP profile at `/.well-known/ucp` tells shopping agents what your checkout supports and how they
can pay, so they can complete a purchase without scraping your cart pages.

**SagBot reads** `/.well-known/ucp` on your site.

## Expose an Agentic Commerce Protocol endpoint

ACP is the checkout protocol behind purchases inside ChatGPT. A discovery document at
`/.well-known/acp.json` points agents at your checkout API. SagBot shows it as advice: adopt it when
you're ready to sell inside assistants.

**SagBot reads** `/.well-known/acp.json` on your site.

## Let agents pay over HTTP with x402

x402 lets an agent pay for a resource in the request itself: the server answers
`402 Payment Required` with the price, the agent pays and retries. It suits digital goods and paid
content more than physical carts.

**SagBot reads** whether your site answers with a `402` and a `Payment-Required` header.

## Mark paid operations for MPP

If your shop has an API, MPP lets you mark which operations cost money with `x-payment-info` in your
OpenAPI file, so agents know the price before they call.

**SagBot reads** `/openapi.json` on your site for operations with `x-payment-info`.

## Describe your shop in structured data

Before any protocol, agents read the page. Organization and Product JSON-LD give them your name,
prices and availability as data rather than as text to interpret.

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Merino wool beanie",
  "offers": {
    "@type": "Offer",
    "price": "29.00",
    "priceCurrency": "EUR",
    "availability": "https://schema.org/InStock"
  }
}
```

**SagBot reads** every JSON-LD block on your site: whether it parses and has typed nodes.

## Serve Markdown to agents

Shopping agents compare many pages per task. Answering `Accept: text/markdown` with clean Markdown
makes your pages cheaper to read than a competitor's, which matters when an agent is choosing where
to look. Add `Vary: Accept` so caches keep the HTML and Markdown versions apart.

**SagBot reads** your site's response to a request with `Accept: text/markdown`.

## See which of these your shop passes

Run a free scan at **[sagbot.app](https://sagbot.app/)** and choose **Shop**. Each failing check
comes with a fix your developer can paste into a coding agent.

---

**Also for your team:** [Developers](developers.md) · [Marketing and PR](marketing.md) · [SEO teams](seo.md) · [Content teams](content.md) · [SaaS and API products](saas.md)
**Guides:** [Improve your GEO](../guides/improve-geo.md) · [AI visibility](../guides/ai-visibility.md) · [All 40 checks](../checks.md)
