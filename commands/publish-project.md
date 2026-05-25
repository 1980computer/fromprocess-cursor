---
name: publish-project
description: Publish a From Process project and return public and embed URLs.
---

# Publish a From Process project

1. Call `get_account_status` and confirm `readyForAgents` with no blockers. If blocked, explain what the user must fix first.
2. Resolve the project:
   - If the user gave a UUID, use it as `formId`
   - If the user gave a name, call `list_projects` and match by name
3. Call `publish_project` with the form UUID.
4. On success, return:
   - `deliverable.publicUrl` — shareable hosted link
   - `deliverable.embedUrl` — iframe embed URL
   - `deliverable.username` and `deliverable.slug`
5. On failure, check plan limits (`NEED_PRO_PLAN`) or missing username and suggest dashboard fixes.

Share drawer in the dashboard also provides HTML iframe and JavaScript embed snippets for the same project.
