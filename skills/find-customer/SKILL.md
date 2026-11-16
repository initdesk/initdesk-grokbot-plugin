---
name: find-customer
description: Find initdesk companies or customer contacts and inspect their support context.
---

Read [workflow boundaries](../../references/workflow-boundaries.md), including organization selection.

1. Search or list companies and customers using the current tool schemas and the user's identifying details. Follow pagination as needed; resolve duplicate names before choosing a record.
2. Retrieve the matching record and only the related support context needed for the request. Keep company UUIDs, customer numeric IDs, and organization IDs distinct.
3. For company ticket filters, use the customer's current company association. Do not describe those results as a historical company membership report. Use customer-specific listing when there is no company association.
4. If asked to change a record, inspect its current values and use the smallest supported update. Attaching members skips customers who already have a company; report attached, skipped, and missing results instead of claiming they were moved.
5. Summarize the verified identity and requested context. Link returned source URLs where available.

Complete when the intended customer or company is resolved, or the remaining ambiguity or access limitation is explicit.
