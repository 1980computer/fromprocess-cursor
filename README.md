# From Process — Cursor plugin

Official [Cursor Marketplace](https://github.com/cursor/plugin-template) plugin for [From Process](https://fromprocess.com): create, publish, and manage forms, waitlists, landing pages, and link-in-bio pages from Cursor via MCP.

Built from the [Cursor plugin template](https://github.com/cursor/plugin-template). The MCP server ships separately as [`@fromprocess/mcp`](https://www.npmjs.com/package/@fromprocess/mcp) (source in [fromprocess.com](https://github.com/1980computer/fromprocess.com) `packages/mcp`).

## What's included

| Component | Purpose |
|-----------|---------|
| **MCP** (`mcp.json`) | Connects to `@fromprocess/mcp` for 24 project/submission tools |
| **Skill** `fromprocess` | Main workflow: create, publish, submissions, analytics |
| **Skill** `headless-integration` | Custom site UIs via public schema + submit API |
| **Rule** `fromprocess-agent` | Agent behavior for account checks, lifecycle, errors |
| **Commands** | `check-account`, `create-waitlist`, `publish-project` |

## Setup

### 1. From Process account (one time)

1. [Sign up](https://fromprocess.com/dashboard/signup) or [sign in](https://fromprocess.com/dashboard/signin)
2. Set a **username** in [Settings](https://fromprocess.com/dashboard/settings)
3. Create an **API key** (`fp_…`) under Settings → API keys

### 2. Install the plugin

Install from the Cursor Marketplace (when published), or clone this repo and load it locally.

### 3. Configure MCP

The plugin ships `mcp.json` pointing at the official server:

```json
{
  "mcpServers": {
    "fromprocess": {
      "command": "npx",
      "args": ["-y", "@fromprocess/mcp"],
      "env": {
        "FROMPROCESS_API_KEY": "fp_your_key_here",
        "FROMPROCESS_API_URL": "https://fromprocess.com"
      }
    }
  }
}
```

Replace `fp_your_key_here` with your API key. For local dev against a cloned site:

```json
"FROMPROCESS_API_URL": "http://localhost:3000"
```

Reload MCP after saving (Settings → MCP, or Developer: Reload Window).

## Try it

After setup, open a new chat and try:

- `/check-account` — verify your account is ready for agents
- "Create a waitlist named Launch list with slug launch-list. Do not publish yet."
- `/publish-project` — publish a draft and get public + embed URLs

More copy-ready prompts: [What to ask your agent](https://fromprocess.com/docs/agentic/what-to-ask)

## MCP tools

Account & projects: `get_account_status`, `list_projects`, `get_project`, `update_project`, `publish_project`

Create: `create_standard_form`, `create_email_signup`, `create_landing_page`, `create_link_in_bio`, `create_from_template`

Submissions: `list_submissions`, `get_submission`, `export_submissions`

Settings & webhooks: `get_project_settings`, `update_project_settings`, `list_webhooks`, `create_webhook`, `update_webhook`, `delete_webhook`

Media & analytics: `upload_page_media`, `get_project_metrics`, `get_published_schema`

Templates: `list_templates`, `get_template`

Full reference: [MCP server docs](https://fromprocess.com/docs/agentic/mcp)

## Development

Validate the plugin manifest before submission:

```bash
node scripts/validate-template.mjs
```

### Local MCP (monorepo dev)

When developing against a local clone of fromprocess.com:

```bash
cd /path/to/fromprocess.com/packages/mcp
npm install && npm run build
```

Point MCP config at `node /absolute/path/to/fromprocess.com/packages/mcp/dist/index.js`.

## Submission checklist

- [ ] Valid `.cursor-plugin/plugin.json` and `marketplace.json`
- [ ] Logo at `assets/logo.svg`
- [ ] Skill and rule frontmatter complete
- [ ] `node scripts/validate-template.mjs` passes
- [ ] Submit repo link to the Cursor team (Slack or `kniparko@anysphere.com`)

## Links

- Site: https://fromprocess.com
- Agentic docs: https://fromprocess.com/docs/agentic
- Getting started: https://fromprocess.com/docs/agentic/getting-started
- Headless API: https://fromprocess.com/docs/headless
- OpenAPI: https://fromprocess.com/api/openapi
- MCP npm: https://www.npmjs.com/package/@fromprocess/mcp
