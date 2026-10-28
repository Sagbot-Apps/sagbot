# Agent readiness for SaaS and API products: API catalog, MCP and OAuth discovery

**Can AI agents discover and connect to your product?** Agents don't browse your docs to find your
API. They look in a few well-known places on your domain: an API catalog, an MCP server card, an A2A
agent card, OAuth metadata and `auth.md`. SagBot shows whether an agent that knows only your domain
can find your API, your MCP server and how to sign in.

[Check your product with SagBot](https://sagbot.app/) · [Full page on sagbot.app](https://sagbot.app/for/saas) · [README](../README.md#who-is-sagbot-for)

## Checklist

| Do this | Where | SagBot check |
|---|---|---|
| Publish an API catalog | `/.well-known/api-catalog` | `agent-api-catalog` |
| Describe your MCP server | `/.well-known/mcp/server-card.json` | `agent-mcp-card` |
| Publish an agent card | `/.well-known/agent-card.json` | `agent-a2a-card` |
| Make sign-in discoverable | `/.well-known/oauth-authorization-server` or `/.well-known/openid-configuration` | `agent-oauth` |
| Say which server issues tokens for your API | `/.well-known/oauth-protected-resource` | `agent-oauth-resource` |
| Explain how agents get credentials | `/auth.md` | `agent-auth-md` (advice) |
| Expose your app's actions as WebMCP tools | your HTML and scripts | `agent-webmcp` (advice) |

Pick **API or app** as the site type when you scan to include these checks.

## Publish an API catalog

An RFC 9727 catalog at `/.well-known/api-catalog` lists your APIs and links each to its OpenAPI
description. One request, and an agent knows what you offer.

```json
{
  "linkset": [{
    "anchor": "https://api.example.com/v1",
    "service-desc": [{ "href": "https://api.example.com/v1/openapi.json", "type": "application/json" }],
    "service-doc": [{ "href": "https://example.com/docs/api", "type": "text/html" }]
  }]
}
```

Serve it as `application/linkset+json`.

**SagBot reads** `/.well-known/api-catalog` on your site and the links in it.

## Describe your MCP server

If you run an MCP server, a server card at `/.well-known/mcp/server-card.json` lets MCP clients find
it and set it up from your domain alone, instead of a user copying a URL from your docs.

**SagBot reads** the MCP server card paths under `/.well-known/` on your site.

## Publish an agent card

If your product is itself an agent, an A2A card at `/.well-known/agent-card.json` tells other agents
what it does and how to talk to it.

**SagBot reads** `/.well-known/agent-card.json` on your site.

## Make sign-in discoverable

Authorization server metadata lets an agent find your sign-in and token endpoints without
hard-coding them. Serve `/.well-known/oauth-authorization-server` or
`/.well-known/openid-configuration`.

**SagBot reads** the OAuth and OpenID Connect metadata documents on your site.

## Say which server issues tokens for your API

RFC 9728 metadata at `/.well-known/oauth-protected-resource` tells an agent which authorization
server to ask for a token before calling your API. MCP servers that require sign-in rely on it.

```json
{
  "resource": "https://api.example.com",
  "authorization_servers": ["https://auth.example.com"]
}
```

**SagBot reads** `/.well-known/oauth-protected-resource` on your site.

## Explain how agents get credentials

An `/auth.md` file says in plain Markdown how an agent registers, which scopes exist and where keys
come from. Shown as advice: it's new, and cheap to add.

**SagBot reads** `/auth.md` on your site.

## Expose your app's actions as WebMCP tools

WebMCP lets an agent running in the browser call your app's actions as tools instead of clicking
through the UI. Register tools with `document.modelContext.registerTool()`, or add `toolname` and
`tooldescription` to key forms.

**SagBot reads** your site's HTML and scripts for registered WebMCP tools or annotated forms. It's advice, not part of the score.

## See which of these your product passes

Run a free scan at **[sagbot.app](https://sagbot.app/)** and choose **API or app**. Each failing
check comes with a fix to paste into a coding agent.

SagBot also checks an agent skills index, an `ai-catalog` and Personal Agent Protocol discovery for
API and app sites. See [all 40 checks](../checks.md#protocol-discovery).

---

**Also for your team:** [Developers](developers.md) · [Marketing and PR](marketing.md) · [SEO teams](seo.md) · [Content teams](content.md) · [Online shops](shops.md)
**Guides:** [Improve your GEO](../guides/improve-geo.md) · [AI visibility](../guides/ai-visibility.md) · [All 40 checks](../checks.md)
