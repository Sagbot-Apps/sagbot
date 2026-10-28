# Technical SEO for AI answers: the basics AI search still stands on

**Which SEO basics do AI answers still depend on?** Most AI answers are built on top of a search
index. If a page can't be indexed, it can't be cited. Status codes, HTTPS, indexability, titles,
canonicals, `hreflang` and JSON-LD are the technical SEO you already know, and SagBot scores them
the way answer engines feel them.

[Check your site with SagBot](https://sagbot.app/) · [Full page on sagbot.app](https://sagbot.app/for/seo) · [README](../README.md#who-is-sagbot-for)

## Checklist

| Do this | SagBot check |
|---|---|
| Answer 200 OK | `seo-status` |
| Serve everything over HTTPS | `seo-https` |
| Don't noindex the page you want cited | `seo-indexability` |
| Write a title that names you and what you do | `seo-title` |
| Give every page a description | `seo-meta-description` |
| Declare one canonical URL | `seo-canonical` |
| Keep hreflang consistent | `seo-hreflang` |
| Add JSON-LD that parses | `geo-jsonld` |

## Answer 200 OK

Crawlers only index pages that answer 200. A chain of redirects, a soft 404 or a 5xx under load all
end the same way: nothing to cite.

```bash
curl -sIL -o /dev/null -w '%{http_code} %{url_effective}\n' https://example.com/
```

**SagBot reads** the status code your site answers with, after redirects.

## Serve everything over HTTPS

Search engines and AI agents prefer secure pages, and some refuse plain HTTP outright.

**SagBot reads** whether your site ends up on HTTPS after redirects.

## Don't noindex the page you want cited

A `noindex` left over from staging keeps a page out of search, and out of every AI answer built on
search. Check both places it can hide:

```bash
curl -s https://example.com/ | grep -i 'name="robots"'
curl -sI https://example.com/ | grep -i '^x-robots-tag'
```

**SagBot reads** the robots meta tag and `X-Robots-Tag` header on your site.

## Write a title that names you and what you do

The `<title>` is the headline search results and AI answers show for you. Keep it specific and under
about 60 characters.

**SagBot reads** your site's `<title>` and its length.

## Give every page a description

A good description is often used as the snippet under your result. Aim for 50 to 160 characters of
plain sentences.

**SagBot reads** your site's meta description and its length.

## Declare one canonical URL

With and without `www`, with and without a trailing slash, with tracking parameters: a canonical link
tells crawlers which URL is the real one, so links and citations add up instead of splitting.

```html
<link rel="canonical" href="https://example.com/">
```

**SagBot reads** the canonical links on your site, and whether there is exactly one.

## Keep hreflang consistent

Broken `hreflang` can surface the wrong language version in search and AI answers. Each language
version should list all the others and itself, with absolute URLs.

```html
<link rel="alternate" hreflang="en" href="https://example.com/">
<link rel="alternate" hreflang="de" href="https://example.com/de">
<link rel="alternate" hreflang="x-default" href="https://example.com/">
```

**SagBot reads** the `hreflang` alternates on your site, including the self-reference.

## Add JSON-LD that parses

Structured data is where SEO and GEO meet: the same JSON-LD that earns rich results gives answer
engines facts they don't have to infer. One broken comma can void a whole block, so validate it.

**SagBot reads** every JSON-LD block on your site: whether it parses and whether its nodes have a `@type`.

## See which of these your site passes

Run a free scan at **[sagbot.app](https://sagbot.app/)**. The Answer-ready score splits into SEO,
GEO and AEO, so you can see where classic SEO stops and the rest begins.

---

**Also for your team:** [Developers](developers.md) · [Marketing and PR](marketing.md) · [Content teams](content.md) · [Online shops](shops.md) · [SaaS and API products](saas.md)
**Guides:** [Improve your GEO](../guides/improve-geo.md) · [AI visibility](../guides/ai-visibility.md) · [All 40 checks](../checks.md)
