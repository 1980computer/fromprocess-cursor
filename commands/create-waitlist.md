---
name: create-waitlist
description: Create a draft email signup / waitlist project on From Process without publishing.
---

# Create a From Process waitlist

1. Run `get_account_status` if the user has not set up From Process yet.
2. Ask for **name** and **slug** if not provided (slug should be lowercase kebab-case).
3. Call `create_email_signup` with:
   - `name` — display name for the project
   - `slug` — URL segment (e.g. `launch-list`)
   - `description` — optional
   - Do **not** set `published: true` unless the user explicitly asks to publish and account status allows it
4. Return `formId` and `deliverable` from the response.
5. Tell the user they can publish later with "Publish my waitlist PROJECT_NAME" or the `publish-project` command.

Example slug URLs after publish: `https://fromprocess.com/{username}/{slug}`
