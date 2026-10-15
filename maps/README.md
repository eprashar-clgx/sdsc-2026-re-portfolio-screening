# Workshop map

Build the Section 2 map in your own CARTO account. The same steps are at the end of the
[notebook](../notebooks/portfolio_screening.ipynb).

## 1. Load the data

You need four files from [`../data/`](../data/). Keep each table's name the same as its file name, and put
all four in the same dataset:

| File | Table name | What it is |
|---|---|---|
| `portfolio_risk.parquet` | `portfolio_risk` | Your 929 properties with climate risk (checkpoint 1) |
| `growth_h3_r9.parquet` | `growth_h3_r9` | 459 H3 cells with growth and risk measures |
| `growth_parcels.parquet` | `growth_parcels` | 918 properties where new development is happening |
| `parcel_essentials.parquet` | `parcel_essentials` | Every parcel in the study area (about 12 MB) |

**Option A · In the browser**

1. Download the four files from this repo's `data/` folder (or use the ones in your clone).
2. In CARTO Workspace, open **Data Explorer** and click **Import data**.
3. Choose a file, then pick the **CARTO Data Warehouse** connection and your **private** dataset. Keep the
   suggested table name if it matches the file name; otherwise type it.
4. Repeat for the other three files.
5. Open one of the new tables and note its full name, shown at the top, e.g.
   `carto-dw-ac-xxxx.private_yourname_xxxx.portfolio_risk`. Everything before the last dot is your **project and
   dataset**.

**Option B · In Claude, with the CARTO MCP server**

1. Connect the CARTO MCP server to Claude: see [`../mcp/carto_on_claude.md`](../mcp/carto_on_claude.md).
2. Ask Claude to import the files from their GitHub URLs (the notebook prints them), e.g. *"Import these four
   files into my CARTO Data Warehouse private dataset, naming each table after its file name, and tell me the
   full table names."*

## 2. Create the map

In Builder, create a map and add the tables as layers: `growth_h3_r9` (cells), `growth_parcels`, and
`portfolio_risk` on top. `parcel_essentials` is optional on the map; the agent can query it either way.

## 3. Add the AI agent

Add an **AI agent** to the map and fill in:

- **Use Case** and **Instructions**: copy them from [`agent_instructions.md`](agent_instructions.md).
- **Capabilities**: turn on **Query sources** so the agent can run SQL.

Leave the **semantic model** empty for now: step 5 adds it.

## 4. Step 5: before and after the semantic model

1. **Before:** ask *"Which of my properties sit in high-growth cells?"* and note the answer and how the agent
   got there.
2. **Add the semantic model:** take [`../semantic_models/portfolio_screening.yaml`](../semantic_models/portfolio_screening.yaml),
   replace every `YOUR_PROJECT.YOUR_DATASET` with your project and dataset (the notebook's last code cell does
   this for you, or use find and replace), and import it in the agent's semantic model settings.
3. **After:** ask the same question again. Expected: 473 of your 929 properties, every time.

## 5. Keep asking

Use the questions in [`prompts.md`](prompts.md); each has the expected answer.
