# MenuNode MCP server

Let an AI agent read a restaurant's real menu and act for a diner: dishes and current prices, allergens
and possible traces, opening hours, table reservations, the walk-in waitlist and orders.
The data comes from the restaurant itself, as it is entered on [MenuNode](https://menunode.com), so prices,
sold-out dishes and special hours are the ones of today.

No API key and no account: the public server is free and open.

- Endpoint: `https://menunode.com/mcp` (Streamable HTTP, no auth)
- Agent reference: https://menunode.com/llms.txt
- Developers (REST API, owner MCP, webhooks): https://menunode.com/en/developers
- OpenAPI 3.1: https://menunode.com/api/v1/openapi.json

<!-- mcp-name: io.github.agustinnb/menunode -->

## Tools

| Tool | What it does |
|---|---|
| `search_restaurants` | Find restaurants by name, city, cuisine, a dish ("vegan pizza"), open now, diet, or near a point |
| `get_restaurant_info` | Hours, address, contact, services, payment options, set menus for groups, FAQ |
| `get_menu` / `get_menu_structured` | The current menu as Markdown or JSON, with EU-14 allergens per dish (`avoid` filters what a diner avoids) |
| `ask_restaurant` | Send the restaurant a question the menu does not answer |
| `quote_group` / `send_event_inquiry` | Price a group meal from the restaurant's own set menus, or send an event inquiry |
| `prepare_order_link` | A link to the restaurant's order page with the cart filled in; the diner confirms and pays there |
| `create_order` / `confirm_order` / `get_order_status` | Place an order for a diner, confirmed with a code emailed to them |
| `list_available_slots` / `check_availability` | Free reservation times for a date and party size |
| `create_reservation` / `confirm_reservation` / `get_reservation` / `change_reservation` / `cancel_reservation` | Book, look up, move or cancel a table |
| `create_availability_alert` / `confirm_availability_alert` | On a full day, email the guest if a time frees up |
| `get_waitlist_status` / `join_waitlist` / `confirm_waitlist` / `get_waitlist_position` / `leave_waitlist` | The walk-in line: wait time, join, position, leave |

Read tools are marked read-only and return structured content. Orders, reservations and waitlist sign-ups
are only placed at the diner's request and confirmed with a 6-digit code sent to the diner's own email.
Allergen answers say what the restaurant declares; they never claim a dish is safe.

## Connect

**Claude Code**

```sh
claude mcp add --transport http menunode https://menunode.com/mcp
```

**Cursor, Windsurf, VS Code and other clients that take a JSON config**

```json
{
  "mcpServers": {
    "menunode": { "url": "https://menunode.com/mcp" }
  }
}
```

## Example

> "Find a restaurant open now with a gluten-free pasta, and book a table for 4 at 8 pm."

The agent calls `search_restaurants` (`{"dish": "gluten-free pasta", "open_now": true}`),
reads the menu with `get_menu_structured`, checks `list_available_slots`, then `create_reservation` and asks the
guest for the code emailed to them to `confirm_reservation`.

## For restaurant owners

A restaurant appears here once it publishes its menu on MenuNode (30 days free, no card): menu with QR codes,
print-ready PDF, orders and reservations with no commission. Owners can also edit their menu from an AI assistant
through the owner server at `https://menunode.com/mcp/owner` with a personal access token.

Made by MenuNode LLC.
