# Writing content that AI answers quote: AEO for content teams

**How do you write pages that AI answers quote?** Answer engines don't quote a page, they quote a
passage: a short, self-contained paragraph that answers a question. Put the answer right under the
heading, phrase some headings as questions, mark them up, show author and date, and make sure the
text is in the HTML. Writing and structure, the part your content team controls.

[Check your site with SagBot](https://sagbot.app/) · [Full page on sagbot.app](https://sagbot.app/for/content) · [README](../README.md#who-is-sagbot-for)

## Checklist

| Do this | SagBot check |
|---|---|
| Put the answer right under the heading | `aeo-concise-answers` |
| Phrase headings as the questions people ask | `aeo-question-headings` (advice) |
| Mark up your questions and answers | `aeo-faq-schema` |
| Show who wrote it and when | `aeo-author-date` |
| Make sure the text is in the HTML | `geo-citable-text` |
| Decide what AI may do with your content | `agent-content-signals` |

## Put the answer right under the heading

The passage answer engines lift is usually the paragraph straight after a heading. Make it the
answer: one to three sentences, 40 to 320 characters, that still make sense on their own.

| Hard to quote | Easy to quote |
|---|---|
| In today's fast-moving landscape, teams everywhere are rethinking how they work. That's why we built a platform that brings everything together… | Acme is invoicing software for freelancers. It sends invoices, chases late payments and files VAT returns in 14 EU countries. |

**SagBot reads** the paragraph after each heading on your site and checks its length.

## Phrase headings as the questions people ask

"Pricing" tells a model little. "How much does Acme cost?" matches what someone types into an
assistant, and the paragraph under it becomes the answer. You don't need every heading to be a
question; a few in the right places do the work.

**SagBot reads** your site's `h1` to `h4` headings and counts the questions. It's advice, not part of the score.

## Mark up your questions and answers

Once a page has question headings, FAQPage markup hands answer engines the same pairs in a form they
don't have to parse. The marked-up text must match what's visible on the page.

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [{
    "@type": "Question",
    "name": "How much does Acme cost?",
    "acceptedAnswer": {
      "@type": "Answer",
      "text": "Acme is free for up to five invoices a month. Unlimited invoices cost 9 euros a month."
    }
  }]
}
```

**SagBot reads** whether your site has question headings and, if so, FAQPage or QAPage JSON-LD with its questions.

## Show who wrote it and when

A named author and a visible date tell an answer engine the page is maintained by someone
accountable. Undated pages lose out to fresher ones on anything time-sensitive. Use
`<time datetime>` for the date so machines can read it too.

```html
<p>By Olena Koval · Updated <time datetime="2026-10-20">20 October 2026</time></p>
```

**SagBot reads** author and date signals on your site: meta tags, schema `author` and `dateModified`, and `<time>` elements.

## Make sure the text is in the HTML

Most AI crawlers don't run JavaScript. If your words only appear after a script runs, the crawler
sees an empty shell. Ask your developers to render the text on the server; you'll know it worked
when the words show up in "view source".

**SagBot reads** the text on your site without running JavaScript: how many words, and whether it has headings.

## Decide what AI may do with your content

Content Signals in `robots.txt` state your terms separately: appear in search (`search`), be used in
AI answers (`ai-input`), be used to train models (`ai-train`). It's an editorial decision as much as
a technical one, so make it on purpose.

```text
User-agent: *
Content-Signal: search=yes, ai-input=yes, ai-train=no
Allow: /
```

**SagBot reads** the `Content-Signal` lines in your `robots.txt`.

## See which of these your site passes

Run a free scan at **[sagbot.app](https://sagbot.app/)**. SagBot checks your site; use the same
rules on every article you publish.

---

**Also for your team:** [Developers](developers.md) · [Marketing and PR](marketing.md) · [SEO teams](seo.md) · [Online shops](shops.md) · [SaaS and API products](saas.md)
**Guides:** [Improve your GEO](../guides/improve-geo.md) · [AI visibility](../guides/ai-visibility.md) · [All 40 checks](../checks.md)
