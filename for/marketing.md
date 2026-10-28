# AI visibility for marketing and PR: get your brand named in AI answers

**Will AI answers recognise and name your brand?** ChatGPT, Perplexity and Google's AI Overviews pick
a handful of sources for every answer. To be one of them, an answer engine first has to be sure who
you are. That depends on one consistent name, Organization data, `sameAs` profiles and good link
previews. Mostly copy and profile work, not code.

[Check your site with SagBot](https://sagbot.app/) · [Full page on sagbot.app](https://sagbot.app/for/marketing) · [README](../README.md#who-is-sagbot-for)

## Checklist

| Do this | SagBot check |
|---|---|
| Say your name the same way everywhere | `geo-entity-name` |
| Say who is behind the site | `geo-entity` |
| Link your official profiles | `geo-sameas` |
| Decide how your link looks when shared | `seo-opengraph` |
| Write the description you want quoted | `seo-meta-description` |

## Say your name the same way everywhere

An answer engine has to be sure that "Acme", "Acme Inc." and "acme.io" are one company before it
credits you. Pick one name and use it in your page title, your social preview and your structured data.

```html
<title>Acme: invoicing for freelancers</title>
<meta property="og:site_name" content="Acme">
<!-- and "name": "Acme" in your Organization JSON-LD -->
```

**SagBot reads** the name in your site's structured data and checks that it matches the `<title>` and `og:site_name`.

## Say who is behind the site

A small block of Organization data (name, URL, logo) tells AI who publishes the site, so it can cite
you by name instead of "a website". A WebSite block alone doesn't say who you are.

**SagBot reads** the JSON-LD on your site for an Organization, Person or similar node.

## Link your official profiles

A `sameAs` list in your Organization data points to your LinkedIn, Crunchbase, Wikipedia or
app-store pages. It's how a model connects the coverage and reviews it has seen elsewhere back to
you, and keeps you apart from companies with similar names.

This is the part PR feeds: every profile and listing you earn is one more link to add.

```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Acme",
  "url": "https://example.com/",
  "logo": "https://example.com/logo.svg",
  "sameAs": [
    "https://www.linkedin.com/company/acme",
    "https://www.crunchbase.com/organization/acme",
    "https://github.com/acme"
  ]
}
```

**SagBot reads** the `sameAs` links in your site's Organization or Person data, and counts them.

## Decide how your link looks when shared

When an assistant, a chat app or a social feed shows your link, it uses your OpenGraph title,
description and image. Without them it guesses, and the guess is rarely your best headline.

```html
<meta property="og:title" content="Acme: invoicing for freelancers">
<meta property="og:description" content="Invoices, payment reminders and VAT returns in 14 EU countries.">
<meta property="og:image" content="https://example.com/og.png">
```

**SagBot reads** `og:title`, `og:description` and `og:image` on your site.

## Write the description you want quoted

The meta description is often the snippet under your result and the summary an assistant falls back
on. Write one or two plain sentences that say what you do and for whom, in 50 to 160 characters.

| Hard to quote | Easy to quote |
|---|---|
| Welcome to Acme! We're passionate about innovation and committed to excellence in everything we do. | Acme is invoicing software for freelancers: invoices, payment reminders and VAT returns in 14 EU countries. |

**SagBot reads** your site's meta description and its length.

## See which of these your site passes

Run a free scan at **[sagbot.app](https://sagbot.app/)**. You get both scores and a fix for every
failing check, ready to hand to your developer.

---

**Also for your team:** [Developers](developers.md) · [SEO teams](seo.md) · [Content teams](content.md) · [Online shops](shops.md) · [SaaS and API products](saas.md)
**Guides:** [Improve your GEO](../guides/improve-geo.md) · [AI visibility](../guides/ai-visibility.md) · [All 40 checks](../checks.md)
