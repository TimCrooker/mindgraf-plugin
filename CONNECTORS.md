# MindGraf Plugin — Required Connectors

This plugin requires the following MCP connections in Cowork.

| Category | MCP Server | Tools Used | Required? |
|----------|-----------|------------|-----------|
| **Host Mac bridge** | Control your Mac | `osascript` | Yes — all brain CLI operations route through this |
| **Email** | Gmail | `gmail_search_messages`, `gmail_read_message`, `gmail_read_thread`, `gmail_create_draft`, `gmail_list_labels` | For email skill |
| **Calendar** | Google Calendar | `gcal_list_events`, `gcal_create_event`, `gcal_update_event`, `gcal_delete_event`, `gcal_find_my_free_time`, `gcal_list_calendars` | For calendar skill |
| **Messaging** | iMessage | `send_imessage`, `read_imessages`, `get_unread_imessages`, `search_contacts` | For messaging skill |

## Setup

1. **Control your Mac** — must be connected for brain CLI access. All `brain` commands execute via `osascript` → `do shell script`.
2. **Gmail, Calendar, iMessage** — connect through Cowork's MCP sidebar. These are optional; skills that depend on them will report the missing connection if invoked.
3. **MindGraf Desktop** — must be running on the host Mac for the brain CLI API to respond.
