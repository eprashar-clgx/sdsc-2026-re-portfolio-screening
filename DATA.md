# Data dictionary

All files cover the same study area: 8 census tracts in south Lee County, Florida (Bonita Springs, Estero,
north Naples). Geometries are WGS84 (EPSG:4326). GeoParquet files open with GeoPandas
(`geopandas.read_parquet`), DuckDB, QGIS and CARTO. CARTO renames each file's `geometry` column to `geom` on
import.

## `growth_h3_r9.parquet` · growth layer (459 cells)

One row per H3 resolution-9 cell (~0.1 km²). Cells with fewer than 5 parcels are left out.

| Column | Meaning |
|---|---|
| `h3` | H3 cell ID (resolution 9) |
| `total_parcels`, `growth_parcels` | Parcels in the cell, and how many are flagged as growth |
| `growth_share` | `growth_parcels / total_parcels` |
| `crs_parcels`, `crs_median` | Parcels with a composite risk score (CRS, 1–100), and their median |
| `k1_total_parcels`, `k1_growth_parcels`, `k1_growth_share`, `k1_crs_median` | Same measures over the cell and its 6 neighbours (smoothed; use these to classify cells) |
| `geometry` | Cell polygon |

`k1_*` counts overlap between neighbouring cells: add up the raw counts, never the `k1_*` ones.

## `growth_parcels.parquet` · growth parcels (918 points)

Cotality Growth Intelligence for every property in a growth stage, one row per property. The key columns:

| Column | Meaning |
|---|---|
| `clip` | Cotality property ID |
| `tract_geoid` | Census tract |
| `h3` | H3 resolution-9 cell the parcel falls in |
| `growth_stage` | Early Growth, Ongoing Growth or Recently Completed |
| `type_of_growth` | Vacant Land, Single Family Residential, Multi-Family, Commercial or Other |
| `geometry` | Parcel centroid |

The other columns are growth signals: recent building permits, new-construction sales, land-use changes,
builder/developer ownership, new CLIPs and nearby-activity counts. The semantic model describes each one.

## `parcel_essentials.parquet` · every parcel in the study area (21,592 rows)

Cotality Parcel Essentials, latest snapshot: identification, situs address, land use, tax assessment, lot and
building size, structure characteristics, hazard proximity, flood zone and the composite risk score (`crs`).
Column names match the Parcel Essentials product, so the semantic model in
[`semantic_models/`](semantic_models/) describes every field.

- **Grain:** one row per parcel polygon and property pair: 15,325 parcels (`parcel_shape_id`) and 21,447
  properties (`clip`). Condo buildings put many properties on one parcel, so count properties with
  `COUNT(DISTINCT clip)`.
- **Geometry:** the parcel polygon, in `parcel_geometry_wkt`.
- **Left out:** owner name, care-of name and the owner's mailing street address.

## `portfolio_messy.csv` · your messy address list (1,130 rows)

| Column | Meaning |
|---|---|
| `input_id` | Row number |
| `address` | Address as typed, with errors |

## `portfolio_truth.csv` · ground truth (1,130 rows)

The answer key for the messy list: use it to check match rates by error type.

| Column | Meaning |
|---|---|
| `input_id`, `address` | As in `portfolio_messy.csv` |
| `error_type` | clean, abbreviation, typo, zip, case_punct, unit, wrong_city or unmatchable |
| `error_detail` | What was changed, e.g. `DR -> Drive` |
| `is_duplicate` | `True` if the row repeats a property already in the list |
| `truth_clip` | The property's real CLIP (empty for unmatchable rows) |
| `clean_address` | The address before errors were added |

## `portfolio_matches.parquet` · cached address lookups (1,130 rows)

The Cotality MCP result for each messy row (tool `clip-find_property_by_address`).

| Column | Meaning |
|---|---|
| `input_id`, `input_address` | As in `portfolio_messy.csv` |
| `match_status` | `matched` or `not_found` |
| `clip` | Cotality property ID |
| `resolved_address` | Standardised address |
| `match_confidence` | Property match score, 0–10 |
| `address_match_code`, `address_match_description` | How the address was matched, e.g. "Matched unique number, postcode changed" |
| `latitude`, `longitude`, `geometry` | Property location (empty when not found) |

## `portfolio_risk.parquet` · checkpoint 1 (929 properties)

One row per resolved property (duplicates collapsed), with Cotality climate risk (tool
`at-get_property_climate_risk`). Scores run from 1 to 100; higher is riskier.

| Column | Meaning |
|---|---|
| `clip` | Cotality property ID |
| `input_ids`, `n_input_rows` | Messy rows that resolved to this property |
| `raw_address`, `error_type` | One of those rows, as typed |
| `resolved_address`, `match_status`, `match_confidence`, `address_match_code`, `address_match_description`, `lat`, `lon` | From the address lookup |
| `h3` | H3 resolution-9 cell of the property's location (joins to `growth_h3_r9`) |
| `risk_score` | Headline score: all perils combined (= `call_score`) |
| `risk_source`, `risk_type_score_group` | Score source, and the property type it's ranked within (SFR, MFR, COM, ...) |
| `call_score`, `cfl_score` | All perils; flood (inland + storm surge) |
| `flxx_score`, `tcfl_score`, `tcwi_score`, `cnfw_score`, `ceq_score` | Inland flood, storm surge, hurricane wind, non-flood weather, earthquake |
| `call_score_ssp2_45_2050`, `call_score_ssp5_85_2050`, `cfl_score_ssp2_45_2050`, `cfl_score_ssp5_85_2050` | 2050 scores under a middle (SSP2-4.5) and a high (SSP5-8.5) emissions scenario |
| `call_aal_ratio`, `cfl_aal_ratio` | Expected annual loss as a share of property value |
| `geometry` | Property location |

Three properties have no climate scores.
