# Connect the CARTO MCP server to Claude

With the CARTO MCP server connected, Claude can explore your CARTO data, run spatial SQL, and create and
edit Builder maps in the conversation. Claude.ai and Claude Desktop also render CARTO maps inline.

Source: [CARTO docs: Claude](https://docs.carto.com/carto-for-agents/connect-your-platform/claude).

## You need

- A CARTO account with the Editor or Admin role (needed to import data and create maps).
- A Claude plan that supports custom connectors.

## 1. Copy your MCP Server URL

In CARTO Workspace, open **Settings → CARTO AI → CARTO MCP Server** (Editors and Admins also find it under
**Developers → Workflow API & MCP Server**). It looks like:

```
https://<region>.api.carto.com/mcp/<account_id>
```

Copy it from the Workspace rather than typing it. If your organization signs in with single sign-on, copy
the **SSO login** URL instead.

## 2. Add it to Claude

**Claude.ai (web)** or **Claude Desktop**:

1. Open **Settings → Connectors**.
2. Click **Add custom connector**.
3. Name it `CARTO` and paste the MCP Server URL.
4. Click **Add**, then **Connect**.
5. Sign in to CARTO when asked.

No OAuth client or token is needed: CARTO recognises Claude automatically.

## 3. Try it

- "List my CARTO connections."
- "Import these four files into my CARTO Data Warehouse private dataset, naming each table after its file
  name, and tell me the full table names: …" (paste the four URLs printed in part 4 of the
  [notebook](../notebooks/portfolio_screening.ipynb)).
- "Show `growth_h3_r9` on a map coloured by `k1_growth_share`."

Optional: install CARTO's Agent Skills plugin so Claude uses the tools more reliably. In Claude, open
**Customize → Plugins → Personal plugins → + → Add marketplace → Add from a repository**, enter
`CartoDB/agent-skills`, then install **carto-skills**.
