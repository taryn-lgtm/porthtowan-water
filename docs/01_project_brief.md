# Project brief

## The question

Is it safe to surf Porthtowan today, and if not, why?

## Background

Porthtowan is a designated bathing water on Cornwall's north coast, served by South West Water. Two storm overflows are linked to it by the Environment Agency ([WaterFit Live](https://www.southwestwater.co.uk/porthtowan)):

| Overflow | 2022 | 2023 | 2024 | 2025 |
| --- | --- | --- | --- | --- |
| Porthtowan Beach No1 pumping station | 1 | 5 | 0 | 16 |
| Porthtowan No2 pumping station | 8 | 27 | 44 | 42 |

South West Water aims for each overflow at a bathing water to spill no more than 10 times a year by 2030.

## Case study: 7 September 2025

The English Interclub Surfing Championships at Porthtowan, with over 140 competitors, were called off mid-contest when lifeguards red-flagged the beach for a sewage discharge ([Surfing England statement via Carve](https://www.carvemag.com/2025/09/english-interclubs-stopped-by-sewage/)).

- What happened in the data: rainfall in the days before, overflow spill timing, tide, and the nearest bathing water samples.
- How unusual was it: how many days each season look like that one?

## Questions by chapter

### 1. Is it safe to go in today?

- How much higher are E. coli and intestinal enterococci in samples taken after a spill than in samples taken without one?
- How much do rainfall, tide, wind and season add?
- Can a model flag a failing sample before the lab result is published, and how often is it wrong?

### 2. Who's to blame?

- How much do ammonia and phosphate rise downstream of each sewage works compared with upstream?
- How do water companies rank on that measured change, compared with what they report?

### 3. Sewage or farming?

- How have nitrate and phosphate changed over 25 years?
- Do seasonal patterns separate sewage-driven sites (worst in summer low flows) from farm-driven sites (worst after winter rain)?

## Stakeholders

| Who | What they need |
| --- | --- |
| Surfers and swimmers | A clear answer on when it's safe to go in |
| Surfers Against Sewage | Independent evidence for campaigns |
| Cornwall Council and the Environment Agency | Where to focus monitoring |
| Hiring managers | Clear evidence of each analytics skill |

## Success measures

- Every dataset ingested by a tested, repeatable script.
- Every cleaning rule logged with rows affected.
- Chapter 1 model catches at least 7 in 10 failing samples on seasons it was not trained on, with its false alarm rate reported.
- Every published number traceable to a query.
- A Cognos dashboard a non-analyst can use without help.

## Known limits

- Bathing water samples are only taken about 20 times a season, May to September, so Porthtowan alone is too small for a model. The model is trained on all English bathing waters and then applied to Porthtowan.
- Overflow monitors don't run 100% of the time. Years with low monitor uptime are flagged rather than read as zero spills.
- Spill counts show that an overflow discharged, not how much sewage it released.
- Correlation between spills and bacteria is not proof that one caused the other; other sources (livestock, birds, road runoff) are discussed.

## Tools

Excel for the web, Python (scripts and notebooks), Databricks Free Edition (SQL, Delta tables, Genie), IBM Cognos Analytics, GitHub Actions, and generative AI as described in [AI_USAGE.md](../AI_USAGE.md).
