# initdesk plugin

Connect Claude, Cursor, and Grok Build to initdesk tickets, customers, companies, and help-center content through the remote initdesk MCP server. Shared skills help select an organization, triage tickets and replies, find customers, and maintain help-center articles.

## Install locally

### Claude

For Claude chat or Cowork, follow the [initdesk connection guide for Claude](https://help.initdesk.com/plugins/claude) to add the remote MCP connector and complete OAuth. The guide connects the server; it does not install the four bundled skills or imply a directory listing.

For local Claude Code testing, clone this repository, review its contents, and run `claude plugin validate /absolute/path/to/initdesk-plugin`, then `claude --plugin-dir /absolute/path/to/initdesk-plugin`. Check the four `initdesk:` skills and the server in `/mcp`; complete OAuth if prompted.

To test the bundle in Claude chat or Cowork before directory publication, upload a ZIP of this plugin folder through **Plugins → Add → Upload plugin**. Include `.claude-plugin/plugin.json`, `.mcp.json`, and `skills/` at the plugin root. Connect initdesk from the plugin's Connectors tab. Reuse an existing connection to the same server URL when offered.

Local installation and custom-connector setup are available independently of directory approval. This repository does not claim an approved Anthropic directory listing.

See [Claude plugin structure and testing](https://claude.com/docs/plugins/build).

### Cursor

Clone this repository into `~/.cursor/plugins/local/initdesk`, restart or reload Cursor, then enable the plugin's initdesk MCP server and choose Connect. Complete the browser OAuth flow with your initdesk account.

See [Cursor plugin installation and local testing](https://cursor.com/docs/plugins).

### Grok Build

Clone this repository, review its contents, then install it with `grok plugin install /absolute/path/to/initdesk-plugin --trust`. The `--trust` flag explicitly trusts the reviewed local package. Start `grok`, open `/plugins` or `/mcps`, enable initdesk, and complete OAuth when prompted. These commands were checked with Grok Build 1.0.77; this version does not accept `--plugin-dir`.

See [Grok Build plugins](https://docs.x.ai/build/features/skills-plugins-marketplaces).

### Grok Bot

In Grok Bot, open **Connect apps → Add a custom connector**. Set the name to `initdesk`, the type to **Remote HTTPS**, and the server URL to `https://mcp.initdesk.com/mcp`. Leave HTTP headers and Client ID empty, then complete OAuth. If initdesk is already connected, use the existing connector.

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
4. For a plugin installation in Claude, Cursor, or Grok Build, verify that the four shared skills load and that the client discovers only one initdesk MCP server for that host. The package carries host-specific manifest directories; they describe the same server, not separate connections.

Repeat the supported checks separately in Claude, Cursor, Grok Bot, and Grok Build. A Grok Bot custom connector provides tools; this package does not establish bundled-skill discovery there. A successful call from a different MCP host does not establish these clients' OAuth compatibility.

## Tools and write behavior

The connected server is the authority for available tools, parameters, permissions, and behavior. This reference records the catalog discovered on 2026-11-16; clients should inspect live tool schemas. No tools are filtered by this package.

Replies can send messages to customers, article publication changes public content, and administrative updates change organization settings. Requesting a draft does not authorize sending or publishing.

The table flags DELETE and full-update operations; an em dash means no special caution is recorded here, not that a tool has no side effects. Consult the live schema for each operation. Full updates can reset omitted fields: fetch the current record and send the intended complete values, or use a `partial_update_*` tool when available. `update_article` preserves omitted article content. For business hours, omitted weekdays are closed and a null or empty PUT body removes the schedule.

| Tool | Write caution |
| --- | --- |
| `attach_company_members` | — |
| `create_article` | — |
| `create_business_hours_exception` | — |
| `create_collection` | — |
| `create_company` | — |
| `create_customer` | — |
| `create_pending_attachment` | — |
| `create_ticket` | — |
| `create_ticket_attachment` | — |
| `create_ticket_message` | — |
| `delete_article` | **Destructive — DELETE** |
| `delete_business_hours` | **Destructive — DELETE** |
| `delete_business_hours_exception` | **Destructive — DELETE** |
| `delete_collection` | **Destructive — DELETE** |
| `delete_company` | **Destructive — DELETE** |
| `list_articles` | — |
| `list_business_hours_exceptions` | — |
| `list_collection_tree` | — |
| `list_collections` | — |
| `list_companies` | — |
| `list_custom_preset_filters` | — |
| `list_customers` | — |
| `list_inboxes` | — |
| `list_organizations` | — |
| `list_tags` | — |
| `list_ticket_messages` | — |
| `list_ticket_statuses` | — |
| `list_tickets` | — |
| `partial_update_article` | — |
| `partial_update_business_hours_exception` | — |
| `partial_update_collection` | — |
| `partial_update_company` | — |
| `partial_update_customer` | — |
| `partial_update_portal_settings` | — |
| `ping` | — |
| `retrieve_article` | — |
| `retrieve_business_hours` | — |
| `retrieve_business_hours_exception` | — |
| `retrieve_collection` | — |
| `retrieve_company` | — |
| `retrieve_custom_preset_filter` | — |
| `retrieve_customer` | — |
| `retrieve_inbox` | — |
| `retrieve_organization` | — |
| `retrieve_portal_favicon_url` | — |
| `retrieve_portal_logo_url` | — |
| `retrieve_portal_settings` | — |
| `retrieve_tag` | — |
| `retrieve_ticket` | — |
| `retrieve_ticket_attachment` | — |
| `retrieve_ticket_message_raw_content` | — |
| `search_articles` | — |
| `search_tickets` | — |
| `unassign_ticket` | — |
| `update_article` | Full update |
| `update_business_hours` | **Full update; empty/null deletes schedule** |
| `update_business_hours_exception` | Full update |
| `update_collection` | Full update |
| `update_company` | Full update |
| `update_customer` | Full update |
| `update_portal_settings` | Full update |

The five DELETE tools remain exposed. Use them only for the user's explicitly intended target and deletion, subject to the host's confirmation policy. The skills do not turn routine triage or upkeep into authorization for deletion.

## Troubleshooting

- Reconnect through OAuth if the session expires; never paste access or refresh tokens into chat or config.
- For permission errors, verify the selected organization and the user's existing initdesk access.
- If a write fails ambiguously, inspect the record before retrying to avoid duplicate replies or articles.
- Report unsupported OAuth callback URIs or client-specific connection failures before changing backend configuration.

## Documentation and support

- [initdesk documentation](https://docs.initdesk.com)
- [initdesk MCP connection guide](https://help.initdesk.com/integrations/mcp)
- [Use initdesk in Claude](https://help.initdesk.com/plugins/claude)
- [Source](https://github.com/initdesk/initdesk-plugin)
- [Privacy policy](https://www.initdesk.com/privacy-policy)
- [MIT license](LICENSE)
- Support: support@initdesk.com
