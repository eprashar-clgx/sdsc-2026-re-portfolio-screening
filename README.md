# Real estate portfolio screening with Cotality and CARTO MCPs

Hands-on workshop, SDSC 2026. You start with a messy list of ~1,100 property addresses in south Lee County,
Florida, and end with a map that shows which of *your* properties sit where new growth meets high climate
risk.

| Section | What you do | Where |
|---|---|---|
| 1 · Clean & enrich | Profile the messy list, resolve every row to a Cotality property ID (CLIP), add climate risk | Jupyter notebook ([`portfolio_screening.ipynb`](notebooks/portfolio_screening.ipynb)) on cached results |
| 2 · Explore & decide | Upload the data to CARTO, place your properties on growth cells, classify a growth × risk 2×2, ask the map's AI agent questions | CARTO Builder ([maps](maps/README.md)) |

The presenters run each step live with AI agents and MCP servers. **Everyone follows along in the notebook**,
which runs the same steps on the full, pre-computed results. If you have Claude, you can also repeat a few of
the live calls yourself: see [Set up Claude](#set-up-claude-optional).

## What's in this repo

| Path | Contents |
|---|---|
| [`data/`](data/) | GeoParquet and CSV files used in both sections; see [DATA.md](DATA.md) |
| [`notebooks/`](notebooks/) | The follow-along notebook: Section 1 on the cached results, prompts to try in Claude, and how to load the data into CARTO |
| [`semantic_models/`](semantic_models/) | The semantic model that tells the map's AI agent how to read the data |
| [`maps/`](maps/README.md) | How to build the map, the agent's Use Case and Instructions, and the prompts |
| [`mcp/`](mcp/) | Guides to connect the [Cotality](mcp/cotality_on_claude.md) and [CARTO](mcp/carto_on_claude.md) MCP servers to Claude |

## Before the session

- A CARTO account, for Section 2.
- One of: Python 3.10 or later and [VS Code](https://code.visualstudio.com) on your computer, or a Google
  account to use Google Colab.
- Optional: Claude in your browser ([claude.ai](https://claude.ai)) with a plan that supports custom connectors.

## Run the notebook

Pick one of the two ways below. The notebook is the same either way: its first cells install anything missing
and find the data.

### Option A · Clone this repo and run it in VS Code

**1. Create the environment** (once). Run the lines for your system in a terminal.

Mac (Terminal):

```bash
git clone https://github.com/eprashar-clgx/sdsc-2026-re-portfolio-screening.git
cd sdsc-2026-re-portfolio-screening
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
```

Windows (PowerShell or Command Prompt):

```powershell
git clone https://github.com/eprashar-clgx/sdsc-2026-re-portfolio-screening.git
cd sdsc-2026-re-portfolio-screening
py -m venv .venv
.venv\Scripts\python -m pip install -r requirements.txt
```

- On Windows, use `py` rather than `python3`: Windows usually has no `python3` command, and typing it may open
  the Microsoft Store. If `py` isn't found, use `python`.
- The environment (`.venv`) holds just five packages from [`requirements.txt`](requirements.txt): pandas,
  PyArrow, shapely, Matplotlib and ipykernel. It installs in under a minute.
- `.venv` is listed in [`.gitignore`](.gitignore), so its files don't show up as changes in source control.

**2. Open the notebook in VS Code**

1. **File → Open Folder** and choose the `sdsc-2026-re-portfolio-screening` folder itself, not the folder
   above it. VS Code only finds `.venv` at the top of the open folder.
2. Install the **Python** and **Jupyter** extensions if VS Code suggests them.
3. Open `notebooks/portfolio_screening.ipynb`, click **Select Kernel** (top right), then **Python
   Environments**, and choose **.venv**.
4. If `.venv` isn't in the list:
   - run **Developer: Reload Window** from the Command Palette (Ctrl+Shift+P on Windows, Cmd+Shift+P on Mac),
     because VS Code may not notice an environment created while it was open; or
   - run **Python: Select Interpreter → Enter interpreter path…** and pick `.venv/bin/python` (Mac) or
     `.venv\Scripts\python.exe` (Windows), then choose the kernel again.

**Prefer the browser?** Add JupyterLab (about 90 more packages, so it's slower to install, especially on
Windows) and start it:

```bash
.venv/bin/python -m pip install jupyterlab           # Windows: .venv\Scripts\python -m pip install jupyterlab
.venv/bin/python -m jupyter lab notebooks/portfolio_screening.ipynb   # Windows: .venv\Scripts\python -m jupyter lab ...
```

### Option B · Copy the code into Google Colab

1. Open [Google Colab](https://colab.research.google.com) and create a new notebook.
2. Copy the cells of [`notebooks/portfolio_screening.ipynb`](notebooks/portfolio_screening.ipynb) into it, in
   order, and run them top to bottom. Copy the code cells at least; the text cells hold the explanations and
   the **Try it in Claude** prompts, so keep the notebook open on GitHub alongside if you skip them.

- **Environment:** Colab already has pandas, PyArrow, shapely and Matplotlib. The first code cell checks for
  them and installs anything missing with `pip`, inside your Colab session only. There's nothing to set up.
- **Data:** the second code cell downloads the files from this repo's `data/` folder into the Colab session.
  Colab deletes them when the session ends, so if you come back later, run the cells again from the top.
- **Uploading to CARTO:** download the files you need from this repo's `data/` folder on GitHub (or from
  Colab's file panel) and import them in CARTO, as the notebook explains.

## Set up Claude (Recommended)

With Claude in your browser you can repeat a few of the presenters' live steps: look up addresses and climate
risk through the Cotality MCP server, and load the data into CARTO through the CARTO MCP server. The notebook
has a **Try it in Claude** box at each step with the exact prompt, the call budget and what to check.

1. **Cotality connector:** follow [`mcp/cotality_on_claude.md`](mcp/cotality_on_claude.md). It uses a
   workshop URL and key that we show on screen.
2. **CARTO connector:** follow [`mcp/carto_on_claude.md`](mcp/carto_on_claude.md). It signs in with your own
   CARTO account.
3. **Calls are limited.** The Cotality key is shared by the whole room, so stick to the prompts in the
   notebook (8 Cotality calls in total) and use the cached results for everything else.

## Section 2: the map

The end of the notebook walks through loading the data into CARTO, either in the browser or through the CARTO
MCP in Claude, and setting up the map's AI agent. The same steps are in [`maps/README.md`](maps/README.md).
