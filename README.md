# Porthtowan Water

**Is it safe to surf Porthtowan today, and if not, why?**

An end-to-end data analytics project on sewage, rainfall and water quality, starting at one Cornish beach and zooming out to every bathing water and river in England. Built with public data from the Environment Agency, the water industry, Open-Meteo and the National Tidal and Sea Level Facility.

> Status: phase 0 (project framing). Each phase lands as its own release.

## Why this beach

Porthtowan is my local surf break. Its No2 pumping station storm overflow spilled 8 times in 2022, 27 times in 2023, 44 times in 2024 and 42 times in 2025, against South West Water's own target of no more than 10 a year by 2030 ([WaterFit Live](https://www.southwestwater.co.uk/porthtowan)). Surfers Against Sewage is based a few miles along the coast in St Agnes.

On 7 September 2025, the English Interclub Surfing Championships at Porthtowan were called off with surfers mid-heat, after lifeguards red-flagged the beach for a sewage discharge ([Surfing England statement via Carve](https://www.carvemag.com/2025/09/english-interclubs-stopped-by-sewage/)). This project starts by rebuilding that day from the data.

## The three questions

1. **Is it safe to go in today?** Bathing water E. coli and enterococci joined to overflow spills, rainfall and tide, with a model that flags a bad water day before the lab result lands.
2. **Who's to blame?** River chemistry upstream and downstream of sewage works across England, ranking water companies on measured harm.
3. **Sewage or farming?** 25 years of nitrate and phosphate data, using seasonal fingerprints to tell sewage pollution apart from farm runoff.

Full brief: [docs/01_project_brief.md](docs/01_project_brief.md)

## Skills map

Every skill from the IBM Data Analyst Professional Certificate, and where to find the evidence.

| Skill | Phase | Evidence |
| --- | --- | --- |
| Framing questions and stakeholders | 0 | [Project brief](docs/01_project_brief.md) |
| Excel: cleaning, lookups, pivots, dashboards | 1 | [excel/](excel/) |
| Python: APIs, pandas, testing, scheduling | 2 | [src/ingest/](src/ingest/), [tests/](tests/) |
| SQL and data modelling in Databricks | 3 | [sql/](sql/) |
| Data cleaning and quality | 4 | [notebooks/](notebooks/) |
| Exploratory analysis and visualisation | 5 | [notebooks/](notebooks/) |
| Statistics and machine learning | 6 | [notebooks/](notebooks/) |
| Dashboards in IBM Cognos Analytics | 7 | [dashboard/](dashboard/) |
| Generative AI, used and checked | all | [AI_USAGE.md](AI_USAGE.md) |
| Storytelling and presentation | 8 | final report (coming) |

## Data sources

| Source | Used for |
| --- | --- |
| [EA Water Quality Explorer](https://www.data.gov.uk/dataset/water-quality-explorer) | River and coastal chemistry |
| [EA Bathing Water Quality](https://environment.data.gov.uk/bwq/profiles/) | Beach E. coli and enterococci |
| [Storm overflow annual returns (EDM)](https://www.data.gov.uk/dataset/event-duration-monitoring-storm-overflows-annual-returns) | Spill counts and hours, 2016 onwards |
| [South West Water WaterFit Live](https://www.southwestwater.co.uk/porthtowan) | Live overflow status at Porthtowan |
| Open-Meteo historical weather and marine | Rainfall, wind, waves |
| National Tidal and Sea Level Facility, Newlyn | Tide heights |

## Repo layout

```
docs/          brief, cleaning log, model card, final report
excel/         phase 1 workbook and screenshots
src/ingest/    one script per data source
sql/           Databricks bronze, silver, gold, query library, ERD
notebooks/     cleaning, EDA, statistics, model
dashboard/     Cognos screenshots, link, walkthrough video
tests/         parser and data quality tests
AI_USAGE.md    every use of AI, checked and credited
```

Independent analysis using public data. Not affiliated with Surfers Against Sewage, South West Water or the Environment Agency.
