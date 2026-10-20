# Prompts for Section 2

Ask these in the map's AI agent, in order. The expected answers come from the workshop data; small differences
in wording are fine, different numbers are not.

Two kinds of prompt:

- **Worked example:** the semantic model includes this question with checked SQL, the way a team would record
  the queries it relies on. The agent should follow it.
- **Open-ended:** no SQL in the semantic model. The agent works out the query from the model's definitions
  (joins, cutoffs, which risk to compare), as it would for any new question. Check its answer against the
  expected one, and ask it to explain its steps if they differ.

| Step | Prompt | In the semantic model | Expected answer |
|---|---|---|---|
| 5 | Which of my properties sit in high-growth cells? Ask once before importing the semantic model, then again after. | Worked example | Before: it depends (the agent guesses the join and the cutoff). After: 474 of 931 properties (238 in Pursue cells, 236 in Watchlist cells); 1 property isn't in any cell |
| 6 | Classify the cells into Pursue, Watchlist, Hold and Reassess, and colour the map by quadrant. | Worked example | Pursue 62 cells, Watchlist 158, Hold 158, Reassess 77, Unclassified 4 |
| 7 | No prompt: turn on the growth parcels layer and click a few parcels. | - | Growth stage and type in the pop-ups |
| 8 | For each quadrant, show the number of cells, my properties and growth parcels, the average growth parcels per cell, and the mix of growth stages. | Worked example | Watchlist: 158 cells, 236 properties, 608 growth parcels (3.85 per cell; 262 early, 107 ongoing, 239 recently completed). Pursue: 62, 238, 276 (4.45). Hold: 158, 222, 9. Reassess: 77, 234, 18 |
| 9 | Which Watchlist properties have a lower property-level climate risk than their cell? | Open-ended | 47 of the 236 Watchlist properties (all-perils score below the portfolio median of 59); 1 is unscored |

More to try in step 9 (all open-ended):

- What drives the climate risk of 10101 Maddox Ln Apt 202?
- Which of my properties have the highest flood score, and which quadrant are they in?
- How does the Watchlist change if I use the 2050 high-emissions score instead of today's?
- What kinds of properties are in my portfolio (land use), and how old are they?
- Where is early-stage growth concentrated?
