# Semantic model

[`portfolio_screening.yaml`](portfolio_screening.yaml) tells the map's AI agent what the data means: the tables,
every column, how they join, how to classify cells, and which measures can be added up. It follows the
[Apache Ossie](https://ossie.apache.org/) standard with CARTO's spatial extension
([CARTO docs](https://docs.carto.com/carto-user-manual/ai-agents/semantic-model)).

## Use it

1. Load the four data files into CARTO, keeping each table's name the same as its file name, as described in
   [`../maps/README.md`](../maps/README.md).
2. Replace every `YOUR_PROJECT.YOUR_DATASET` in the file with the project and dataset of your tables. The last
   code cell of the [notebook](../notebooks/portfolio_screening.ipynb) does this for you.
3. In step 5, after asking the agent once without it, import the file in the agent's semantic model settings
   and ask again.

## What it covers

| Dataset | Table | Grain |
|---|---|---|
| `portfolio_risk` | `portfolio_risk` | Your 931 properties, with Cotality climate risk |
| `growth_h3_r9` | `growth_h3_r9` | 459 H3 resolution-9 cells |
| `growth_parcels` | `growth_parcels` | 918 properties in a growth stage |
| `parcel_essentials` | `parcel_essentials` | Every parcel and property in the study area |

The rules it pins down:

- **Joins:** properties and growth parcels join to cells on `h3`; properties and growth parcels join to Parcel
  Essentials on `clip`. Properties never join to growth parcels directly: none of your properties is a growth
  parcel.
- **Growth class:** `k1_growth_share` above 0.013 (the study-area median) is high growth.
- **Risk class:** `k1_crs_median` at or above 61 (the study-area median) is high risk.
- **Quadrants:** high growth / low risk = **Pursue**, high growth / high risk = **Watchlist**, low growth / low
  risk = **Hold**, low growth / high risk = **Reassess**.
- **Additive measures:** sum `total_parcels` and `growth_parcels`; never sum `k1_*` columns (neighbouring cells
  overlap, so parcels would be counted up to 7 times).
- **Two levels of risk:** a property's own climate risk (`risk_score`) and its cell's composite risk score are
  different measures; compare their classes, not their values.

The model is built from the Cotality Parcel Essentials and Growth Intelligence models, plus the workshop rules
above. It includes worked examples whose SQL has been run against the workshop data.
