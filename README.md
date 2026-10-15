# Real estate portfolio screening with Cotality and CARTO MCPs

Hands-on workshop, SDSC 2026. You start with a messy list of ~1,100 property addresses in south Lee County,
Florida, and end with a map that shows which of *your* properties sit where new growth meets high climate
risk.

| Section | What you do | Where |
|---|---|---|
| 1 · Clean & enrich | Profile the messy list, resolve every row to a Cotality property ID (CLIP), add climate risk | Jupyter notebook ([`portfolio_screening.ipynb`](notebooks/portfolio_screening.ipynb)) on cached results |
| 2 · Explore & decide | Upload the data to CARTO, place your properties on growth cells, classify a growth × risk 2×2, ask the map's AI agent questions | CARTO Builder ([maps](maps/README.md)) |

The presenters run each step live with AI agents and MCP servers. You run the same steps on the full,
pre-computed results, so nothing depends on live API access during the session.

## What's in this repo

| Path | Contents |
|---|---|
| [`data/`](data/) | GeoParquet and CSV files used in both sections; see [DATA.md](DATA.md) |
| [`notebooks/`](notebooks/) | The follow-along notebook: Section 1 on the cached results, then how to load the data into CARTO |
| [`semantic_models/`](semantic_models/) | The semantic model that tells the map's AI agent how to read the data |
| [`maps/`](maps/README.md) | How to build the map, the agent's Use Case and Instructions, and the prompts |
| [`mcp/`](mcp/) | Guides to connect the [CARTO](mcp/carto_on_claude.md) and [Cotality](mcp/cotality_on_claude.md) MCP servers to Claude |

## Before the session

- A CARTO account, for Section 2.
- Either Python 3.10 or later on your computer, or a Google account to use Google Colab.
- Optional, to redo the live MCP steps or upload the data from a chat: a Claude account with custom
  connectors.

## Run the notebook

Pick one of the two ways below. The notebook is the same either way: its first cells install anything missing
and find the data.

### Option A · Clone this repo

```bash
git clone https://github.com/eprashar-clgx/sdsc-2026-re-portfolio-screening.git
cd sdsc-2026-re-portfolio-screening
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab notebooks/portfolio_screening.ipynb
```

- **Environment:** the virtual environment (`.venv`) keeps the packages in
  [`requirements.txt`](requirements.txt) (pandas, GeoPandas, PyArrow, Matplotlib, JupyterLab) separate from
  the rest of your Python. Activate it again (`source .venv/bin/activate`) whenever you come back.
- **Data:** the notebook reads the files in `data/` from your clone; nothing is downloaded.

### Option B · Copy the code into Google Colab

1. Open [Google Colab](https://colab.research.google.com) and create a new notebook.
2. Copy the cells of [`notebooks/portfolio_screening.ipynb`](notebooks/portfolio_screening.ipynb) into it, in
   order (code cells are enough; the text cells are explanations), and run them top to bottom.

- **Environment:** Colab already has pandas, GeoPandas, PyArrow and Matplotlib. The first code cell checks
  for them and installs anything missing with `pip`, inside your Colab session only. There's nothing to set up.
- **Data:** the second code cell downloads the files from this repo's `data/` folder into the Colab session.
  Colab deletes them when the session ends, so if you come back later, run the cells again from the top.
- **Uploading to CARTO:** download the files you need from this repo's `data/` folder on GitHub (or from
  Colab's file panel) and import them in CARTO, as the notebook explains.

## Section 2: the map

The end of the notebook walks through loading the data into CARTO, either in the browser or through the CARTO
MCP in Claude, and setting up the map's AI agent. The same steps are in [`maps/README.md`](maps/README.md).
