# Connect the Cotality MCP server to Claude

With the Cotality MCP server connected, Claude can standardize addresses, find a property's CLIP, and fetch
property data such as climate risk, in the conversation. In the workshop you use it for the **Try it in
Claude** boxes in the [notebook](../notebooks/portfolio_screening.ipynb).

## You need

- Claude in your browser ([claude.ai](https://claude.ai)) on a plan that supports custom connectors. On Pro and
  Max plans you add the connector yourself. On Team and Enterprise plans an Owner has to add it to the
  organization first; you then find it under **Customize → Connectors** and click **Connect**.
- The workshop **MCP server URL** and **key**. We show both on screen during the session.

## 1. Add the connector

1. In Claude, go to **Customize → Connectors**.
2. Click **+ Add**, then **Add custom connector**.
3. Name it `Cotality` and paste the **MCP server URL** from the screen. Click **Continue**.
4. Under **Authentication**, choose **No sign in**.
5. Open **Request headers** and add the header shown on screen (usually `authorization`). As its value,
   enter the key exactly as shown, **including** the word `Bearer` and the space after it:
   `Bearer sk-…`. Claude sends the value exactly as you type it and doesn't add `Bearer` for you.
6. Mark the header **Required**, then click **Add**.

You can't edit a connector's authentication afterwards. If the key was mistyped, remove the connector and add
it again.

## 2. Turn it on in a chat

Start a new chat, click **+ → Connectors**, and turn **Cotality** on. Turn off connectors you don't need, so
Claude doesn't reach for the wrong tool. The default Sonnet model is fine and fastest.

## 3. Use it, within the budget

The workshop key is shared by everyone in the room. Use the prompts in the notebook's **Try it in Claude**
boxes: **6 calls in total** (5 address lookups and 1 climate risk lookup). Please don't ask Claude to work
through the whole address list; the notebook's cached results cover that.

- **Address lookups:** the prompts use `li-recognize_address`, which returns the standardized address and CLIP.
  `clip-find_property_by_address` returns the same, plus owner names.
- **Climate risk:** `at-get_property_climate_risk` returns about 170 KB per property (12 perils, today and
  2030/2040/2050 under four emissions scenarios). Ask for one property per call, and ask Claude to report just
  the scores you need.

## Tools you'll see

| Tool | What it does |
|---|---|
| `li-recognize_address` | Standardizes one address and returns its best CLIP match |
| `clip-find_property_by_address` | Looks up a property and its CLIP from a full address |
| `clip-find_property_by_clip` | Looks up a property from its CLIP |
| `at-get_property_climate_risk` | Climate risk scores for CLIPs: 12 perils, today and 2030/2040/2050 under four emissions scenarios |
| `at-get_property_roof_age`, `at-get_property_analytics` | Roof age; climate risk and roof age together |
| `pd-get_property_characteristics`, `pv-get_property_value` | Property characteristics; valuation |
| `pr-*` | Market trends: home price index, listings, rentals, sales |
