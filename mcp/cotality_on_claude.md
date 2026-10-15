# Connect the Cotality MCP server to Claude

With the Cotality MCP server connected, Claude can standardise addresses, find a property's CLIP, and fetch
property data such as climate risk, roof age and valuation, in the conversation.

> **Draft.** Access details are still being confirmed with Cotality; every `TBC` below needs a value
> before this guide is usable.

## You need

- Cotality MCP access. TBC: how attendees get it (trial account, workshop credentials, or contact).
- A Claude plan that supports custom connectors.

## 1. Add the server to Claude

1. In Claude.ai or Claude Desktop, open **Settings → Connectors**.
2. Click **Add custom connector**.
3. Name it `Cotality` and paste the MCP Server URL: TBC.
4. TBC: under **Advanced settings**, enter the OAuth Client ID provided by Cotality (if required).
5. Click **Add**, then **Connect**, and sign in when asked.

## 2. Tools you'll see

| Tool | What it does |
|---|---|
| `li-recognize_address` | Standardises one address and returns its best CLIP match |
| `clip-find_property_by_address` | Looks up a property and its CLIP from a full address |
| `clip-find_property_by_clip` | Looks up a property from its CLIP |
| `at-get_property_climate_risk` | Climate risk scores for up to 25 CLIPs: 12 perils, today and 2030/2040/2050 under four emissions scenarios |
| `at-get_property_roof_age`, `at-get_property_analytics` | Roof age; climate risk and roof age together |
| `pd-get_property_characteristics`, `pv-get_property_value` | Property characteristics; valuation |
| `pr-*` | Market trends: home price index, listings, rentals, sales |

## 3. Try it

Repeat the workshop's live steps with the same addresses:

- "Clean up these addresses and find their CLIPs: …" (paste a few rows from `data/portfolio_messy.csv`)
- "Get the climate risk for CLIP … and summarise the all-perils, flood and hurricane wind scores."

The climate risk response is large (about 170 KB per property), so ask for one property at a time and ask
Claude to summarise it.
