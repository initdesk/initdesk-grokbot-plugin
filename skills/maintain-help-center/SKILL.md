---
name: maintain-help-center
description: Search, draft, update, or publish initdesk help-center articles and organize their collections.
---

Read [workflow boundaries](../../references/workflow-boundaries.md), including organization selection and publishing authorization.

1. Search existing articles before creating one. Read the relevant article and collection tree to find the right destination and avoid duplicates.
2. Gather the current product facts or source tickets needed for the change. Omit personal details, private ticket content, credentials, and internal-only information from public articles.
3. Draft concise Markdown grounded in those sources. Keep `initdesk` lowercase and use the requested language naturally.
4. For a requested new draft, use `create_article` with `status: draft`; for an existing article, inspect its status first. Editing a published article can change public content, so keep a draft-only request in chat unless a supported unpublished revision workflow exists.
5. Publish only when authorized. Verify the destination, final content, and current schema; prefer `partial_update_article` for selected fields so omitted content stays intact. Collection deletion is a separate action, not a way to tidy the article tree.
6. Read the resulting article when needed to verify its content and publication status. Report the article's ID, status, and usable source URL.

Complete with the requested draft or verified article change, including any unresolved publication limitation.
