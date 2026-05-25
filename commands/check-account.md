---
name: check-account
description: Check From Process account readiness, plan, and blockers before creating or publishing projects.
---

# Check From Process account

1. Call `get_account_status` via the From Process MCP server.
2. Report to the user:
   - Username and whether it is set
   - Plan tier and `canPublish`
   - Whether an API key exists and has been used (`hasApiKey`, `apiKeyUsed`)
   - `readyForAgents` and any `blockers`
3. If blockers exist, give concrete next steps in the dashboard:
   - Missing username → https://fromprocess.com/dashboard/settings
   - No API key → Settings → API keys
   - Plan limit → explain upgrade path if publish is blocked
4. If MCP is not connected, help the user set `FROMPROCESS_API_KEY` in plugin MCP config or run the HTTP equivalent: `GET /api/user/agent-readiness` with Bearer auth.
