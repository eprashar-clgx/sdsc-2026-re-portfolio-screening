# Connect the CARTO MCP server to Claude

With the CARTO MCP server connected, Claude can explore your CARTO data, run spatial SQL, and create and
edit Builder maps in the conversation. Claude.ai and Claude Desktop also render CARTO maps inline.

Source: [CARTO docs: Claude](https://docs.carto.com/carto-for-agents/connect-your-platform/claude).

## You need

- A CARTO account with the Editor or Admin role (needed to import data and create maps).
- Claude in your browser ([claude.ai](https://claude.ai)) on a plan that supports custom connectors. On Team
  and Enterprise plans an Owner adds the connector to the organization first; you then find it under
  **Customize → Connectors** and click **Connect**.

## 1. Copy your MCP Server URL

In CARTO Workspace, open **Settings → CARTO AI → CARTO MCP Server** (Editors and Admins also find it under
**Developers → Workflow API & MCP Server**). It looks like:

```
https://<region>.api.carto.com/mcp/<account_id>
```

Copy it from the Workspace rather than typing it. If your organization signs in with single sign-on, copy
the **SSO login** URL instead.

## 2. Add it to Claude

1. In Claude, go to **Customize → Connectors**.
2. Click **+ Add**, then **Add custom connector**.
3. Name it `CARTO`, paste the MCP Server URL and click **Continue**.
4. Keep the authentication settings Claude detects (sign in with OAuth, Claude's published identity) and
   click **Add**.
5. Click **Connect** and sign in to CARTO when asked.

No OAuth client, token or request header is needed: CARTO recognises Claude automatically. This connector uses
your own CARTO account, not the shared workshop key.

To use it, start a chat, click **+ → Connectors**, and turn **CARTO** on.

## 3. Try it

- "List my CARTO connections."
- The import prompt in part 4 of the [notebook](../notebooks/portfolio_screening.ipynb), which loads the four
  workshop files into your CARTO Data Warehouse.
- "Show `growth_h3_r9` on a map coloured by `k1_growth_share`."

Optional: install CARTO's Agent Skills plugin so Claude uses the tools more reliably. In Claude, open
**Customize → Plugins → Personal plugins → + → Add marketplace → Add from a repository**, enter
`CartoDB/agent-skills`, then install **carto-skills**.
