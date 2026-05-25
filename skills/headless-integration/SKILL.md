---
name: headless-integration
description: Build custom form UIs on external sites using From Process published schema and public submit API. Use when embedding forms in React/Next.js sites, fetching schema JSON, or POSTing submissions without the hosted page.
---

# From Process headless integration

Use From Process as the form engine while your site owns layout and CSS.

## When to use

- User wants a form on their own website, not a hosted fromprocess.com page
- User asks for schema JSON, public submit API, or CORS-enabled form endpoints
- User is building React/Next.js/Vue components that POST to From Process

## Prerequisites

- A **published** project with username and slug (create via MCP or dashboard first)
- No API key needed for public schema/submit routes

## Flow

1. **Fetch schema** — `get_published_schema(username, slug)` or:
   ```
   GET https://fromprocess.com/api/public/forms/{username}/{slug}/schema
   ```
2. **Render** — Only fields with `headless.support` marked native. Use field `id` as input name/key.
3. **File uploads** — For file/signature fields:
   ```
   POST /api/public/forms/{username}/{slug}/upload
   multipart: file + fieldId
   ```
   Include returned key in submit payload.
4. **Submit** — JSON keyed by field id:
   ```
   POST /api/public/forms/{username}/{slug}/submit
   Content-Type: application/json
   { "field-email": "user@example.com", "field-message": "Hello" }
   ```

## MCP vs HTTP

| Step | MCP tool | HTTP |
|------|----------|------|
| Read schema | `get_published_schema` | `GET …/schema` |
| Submit | Use HTTP (public, no auth) | `POST …/submit` |
| Upload | Use HTTP (public, no auth) | `POST …/upload` |

## Example agent tasks

- "Fetch the published headless schema for username USERNAME and slug SLUG and explain the fields."
- "Give me a minimal React example that loads my From Process form USERNAME/SLUG and submits responses."

## Field types

Check `headless.support` per field in the schema response. Unsupported types may need the hosted page or embed iframe instead.

## References

- Docs: https://fromprocess.com/docs/headless
- OpenAPI: https://fromprocess.com/api/openapi
- Agentic overview: https://fromprocess.com/docs/agentic

## CORS

Public headless routes include `Access-Control-Allow-Origin` for browser `fetch` from customer domains.

## Payments

If the form has inline payments, use `POST …/payment-intent` before submit (see OpenAPI).
