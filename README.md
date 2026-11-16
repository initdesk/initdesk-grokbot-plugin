# initdesk plugin

Connect Cursor and Grok Build to initdesk tickets, customers, companies, and help-center content through the remote initdesk MCP server. Shared skills help select an organization, triage tickets and replies, find customers, and maintain help-center articles.

This is the initial plugin package. Marketplace approval and end-to-end client compatibility must be verified before a marketplace release is announced. Grok Bot uses the Cursor Marketplace; its OAuth connection and listing visibility need separate verification.

## Install locally

### Cursor

Clone this repository into `~/.cursor/plugins/local/initdesk`, restart or reload Cursor, then enable the plugin's initdesk MCP server and choose Connect. Complete the browser OAuth flow with your initdesk account.

See [Cursor plugin installation and local testing](https://cursor.com/docs/plugins).

### Grok Build

Clone this repository, review its contents, then install it with `grok plugin install /absolute/path/to/initdesk-plugin --trust`. The `--trust` flag explicitly trusts the reviewed local package. Start `grok`, open `/plugins` or `/mcps`, enable initdesk, and complete OAuth when prompted. These commands were checked with Grok Build 1.0.77; this version does not accept `--plugin-dir`.

See [Grok Build plugins](https://docs.x.ai/build/features/skills-plugins-marketplaces).

### Grok Bot

For an initial connection test, add a custom MCP connector using `https://mcp.initdesk.com/mcp`, then complete OAuth. Exact controls and OAuth callback compatibility must be checked in the current Grok Bot client. Marketplace installation is pending review.

## Connection and credentials

| Destination | Purpose |
| --- | --- |
| `https://mcp.initdesk.com/mcp` | Remote MCP tools over Streamable HTTP |
| `https://mcp.initdesk.com/.well-known/oauth-protected-resource/mcp` | OAuth protected-resource discovery |
| `https://auth.initdesk.com` | initdesk sign-in, authorization, token exchange, and dynamic client registration |

The client discovers OAuth metadata from the MCP authentication challenge. initdesk supports OAuth 2.1 with PKCE S256 and dynamic registration for public clients. No API key, client ID, client secret, or bearer token is bundled or needs to be pasted into this repository. Complete sign-in in the browser; the host client manages the OAuth connection.

Tool calls run with the connected initdesk user's existing access. Selecting an organization is required for organization-scoped operations and does not expand permissions. The MCP service processes the selected requests and returns their results to the host client. The plugin adds no telemetry, local executable, hooks, or dependencies.

Dynamic client registration is rate-limited. Repeated reconnects can exhaust registration capacity; avoid repeatedly registering clients while troubleshooting.

## First connection check

1. Run `ping` and verify its successful response.
2. Run `list_organizations`; select the intended organization's internal numeric ID.
3. Run one organization-scoped read, such as `list_inboxes`, with that ID.
4. Verify that the four shared skills load and that the client discovers only one initdesk MCP server despite the two manifest directories.

Repeat this check separately in Cursor, Grok Bot, and Grok Build. A successful call from a different MCP host does not establish these clients' OAuth compatibility.

## Tools and write behavior

The connected server is the authority for available tools, parameters, permissions, and behavior. This reference records the catalog discovered on 2026-11-16; clients should inspect live tool schemas. No tools are filtered by this package.

Replies can send messages to customers, article publication changes public content, and administrative updates change organization settings. Requesting a draft does not authorize sending or publishing.

| Tool | Effect |
| --- | --- |
| `attach_company_members` | Write |
| `create_article` | Write |
| `create_business_hours_exception` | Write |
| `create_collection` | Write |
| `create_company` | Write |
| `create_customer` | Write |
| `create_pending_attachment` | Write |
| `create_ticket` | Write |
| `create_ticket_attachment` | Write |
| `create_ticket_message` | Write |
| `delete_article` | **Destructive — DELETE** |
| `delete_business_hours` | **Destructive — DELETE** |
| `delete_business_hours_exception` | **Destructive — DELETE** |
| `delete_collection` | **Destructive — DELETE** |
| `delete_company` | **Destructive — DELETE** |
| `list_articles` | Read |
| `list_business_hours_exceptions` | Read |
| `list_collection_tree` | Read |
| `list_collections` | Read |
| `list_companies` | Read |
| `list_custom_preset_filters` | Read |
| `list_customers` | Read |
| `list_inboxes` | Read |
| `list_organizations` | Read |
| `list_tags` | Read |
| `list_ticket_messages` | Read |
| `list_ticket_statuses` | Read |
| `list_tickets` | Read |
| `partial_update_article` | Write |
| `partial_update_business_hours_exception` | Write |
| `partial_update_collection` | Write |
| `partial_update_company` | Write |
| `partial_update_customer` | Write |
| `partial_update_portal_settings` | Write |
| `ping` | Read |
| `retrieve_article` | Read |
| `retrieve_business_hours` | Read |
| `retrieve_business_hours_exception` | Read |
| `retrieve_collection` | Read |
| `retrieve_company` | Read |
| `retrieve_custom_preset_filter` | Read |
| `retrieve_customer` | Read |
| `retrieve_inbox` | Read |
| `retrieve_organization` | Read |
| `retrieve_portal_favicon_url` | Read |
| `retrieve_portal_logo_url` | Read |
| `retrieve_portal_settings` | Read |
| `retrieve_tag` | Read |
| `retrieve_ticket` | Read |
| `retrieve_ticket_attachment` | Read |
| `retrieve_ticket_message_raw_content` | Read |
| `search_articles` | Read |
| `search_tickets` | Read |
| `unassign_ticket` | Write |
| `update_article` | Write |
| `update_business_hours` | Write |
| `update_business_hours_exception` | Write |
| `update_collection` | Write |
| `update_company` | Write |
| `update_customer` | Write |
| `update_portal_settings` | Write |

The five DELETE tools remain exposed. Use them only for the user's explicitly intended target and deletion, subject to the host's confirmation policy. The skills do not turn routine triage or upkeep into authorization for deletion.

## Troubleshooting

- Reconnect through OAuth if the session expires; never paste access or refresh tokens into chat or config.
- For permission errors, verify the selected organization and the user's existing initdesk access.
- If a write fails ambiguously, inspect the record before retrying to avoid duplicate replies or articles.
- Report unsupported OAuth callback URIs or client-specific connection failures before changing backend configuration.

## Documentation and support

- [initdesk documentation](https://docs.initdesk.com)
- [initdesk MCP connection guide](https://help.initdesk.com/integrations-notifications/initdesk-mcp-server)
- [Source](https://github.com/initdesk/initdesk-plugin)
- [MIT license](LICENSE)
- Support: support@initdesk.com
