---
name: fromprocess
description: Create, publish, and manage From Process forms, waitlists, landing pages, and link-in-bio pages via MCP. Use when the user mentions From Process, fromprocess.com, waitlists, form publishing, submissions, or headless forms.
---

# From Process

Official Cursor integration for [From Process](https://fromprocess.com) — forms, email signup, landing pages, and link in bio from one account.

## When to use

- User wants to create a form, waitlist, landing page, or link in bio
- User asks to publish, update, or share a From Process project
- User wants to read or export form submissions
- User is building a custom site UI against a published From Process form (headless)
- User asks about From Process account setup, API keys, or MCP configuration

## Prerequisites

1. [Sign up or sign in](https://fromprocess.com/dashboard/signin)
2. Set a **username** in [Settings](https://fromprocess.com/dashboard/settings)
3. Create an **API key** (`fp_…`) in Settings → API keys
4. Install this plugin (includes MCP) or add `@fromprocess/mcp` manually

Set `FROMPROCESS_API_KEY` in your environment or MCP config. For local dev against a clone: `FROMPROCESS_API_URL=http://localhost:3000`.

## Workflow

### 1. Check account

Call `get_account_status` first when publishing or when the user is new. Report `readyForAgents` and any `blockers`.

### 2. Create (draft by default)

| Goal | Tool |
|------|------|
| Contact form | `create_standard_form` |
| Waitlist | `create_email_signup` |
| Landing page | `create_landing_page` |
| Link in bio | `create_link_in_bio` |
| From template | `list_templates` → `create_from_template` |

Pass `name`, `slug`, optional `description`, optional `sections`. Omit `published` unless the user wants live links immediately.

### 3. Edit

- `get_project` / `update_project` for name, slug, sections, theme
- `upload_page_media` for banner (`banner`) or profile (`profile`) images — local file path required
- `get_project_settings` / `update_project_settings` for notifications, confirmation, publish flag

### 4. Publish

`publish_project` with form UUID, or `update_project_settings` with `published: true`. Return `deliverable.publicUrl` and `deliverable.embedUrl` to the user.

### 5. Submissions and webhooks (Pro)

- `list_submissions`, `get_submission`, `export_submissions`
- `list_webhooks`, `create_webhook`, `update_webhook`, `delete_webhook`

### 6. Analytics

`get_project_metrics` with form UUID and optional `days` (7, 30, or 90).

## Example prompts

Copy these into chat (no need to name tools):

- "Use From Process to check my account status and tell me if I am ready for agents."
- "Create a waitlist named Launch list with slug launch-list. Do not publish yet."
- "Publish my From Process project PROJECT_NAME and give me the public link and embed link."
- "Export all submissions from PROJECT_NAME as JSON and summarize them."
- "Upload /path/to/hero.jpg as the banner on my landing page PROJECT_NAME, then publish."

Full prompt library: https://fromprocess.com/docs/agentic/what-to-ask

## Headless integration

For custom UIs on the user's site:

1. `get_published_schema(username, slug)`
2. Build native inputs from schema `sections` and `headless.support`
3. Submit to public API keyed by field id

See skill `headless-integration` and https://fromprocess.com/docs/headless.

## All MCP tools

`get_published_schema`, `get_account_status`, `list_projects`, `get_project`, `update_project`, `create_email_signup`, `create_landing_page`, `create_link_in_bio`, `create_standard_form`, `publish_project`, `list_submissions`, `get_submission`, `export_submissions`, `upload_page_media`, `get_project_settings`, `update_project_settings`, `list_webhooks`, `create_webhook`, `update_webhook`, `delete_webhook`, `list_templates`, `get_template`, `create_from_template`, `get_project_metrics`

## Troubleshooting

| Issue | Action |
|-------|--------|
| MCP auth failure | Verify `FROMPROCESS_API_KEY` in config; create new key in dashboard |
| Cannot publish | Run `get_account_status`; check username and plan |
| `NEED_PRO_PLAN` | Export and webhooks require Pro |
| Upload fails | Confirm file exists on MCP host machine; use correct field id |

Docs: https://fromprocess.com/docs/agentic · MCP package: `@fromprocess/mcp`
