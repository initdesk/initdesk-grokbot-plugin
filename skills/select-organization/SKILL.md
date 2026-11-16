---
name: select-organization
description: Resolve the organization for initdesk work when starting a task or switching organizations.
---

Read [workflow boundaries](../../references/workflow-boundaries.md).

1. Call `list_organizations`, using a name search when the user supplied an organization name.
2. Match the user's intended organization. If several results could match, ask the user to choose before accessing organization records. If only one organization is available and the user gave no conflicting instruction, use it.
3. Carry its internal numeric `id` into every organization-scoped call. A customer-visible `public_id` is not a substitute.
4. State the selected organization briefly when beginning work or switching.

Complete when the selected organization's name and internal ID are unambiguous. If there is no accessible match, report that limitation.
