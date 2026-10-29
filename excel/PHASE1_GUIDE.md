# Phase 1 guide: Porthtowan in Excel

**Goal:** answer a first, small version of the big question in Excel for the web: *are Porthtowan's water samples worse in years, and on days, with more sewage spills and rain?*

**Why Excel first:** it shows the IBM Excel skills on a dataset small enough to check by eye. It also shows where Excel runs out of road, which is why phases 2 and 3 move to Python and Databricks.

**Time:** two to four sessions. Tick each box as you go and take a screenshot at every 📸.

---

## Part A: Get the data (by hand, in your browser)

Save every file untouched in a folder called `raw` on your computer. Note the date you downloaded each one; you'll need it for the README tab.

- [ ] **A1. Recent bathing water samples.** Go to [Swimfo](https://environment.data.gov.uk/bwq/profiles/), search *Porthtowan*, open its profile and find the sample results (E. coli and intestinal enterococci). Download every season it offers. Note the bathing water ID shown on the page (it starts `ukk`).
- [ ] **A2. Older samples (optional, for the long view).** Download [Historical Bathing Water Quality 1988 to 2014](https://www.data.gov.uk/dataset/145c9f49-e671-4821-ad10-7fe8154248f3/historical-bathing-water-quality-at-designated-beaches-1988-2014) (one big xlsx for every beach). You'll filter it to Porthtowan in Part C.
- [ ] **A3. Spill counts.** From the [storm overflow annual returns](https://www.data.gov.uk/dataset/19f6064d-7356-466f-844e-d20ea10ae9fd/event-duration-monitoring-storm-overflows-annual-returns), download the zip for each year 2020 to 2025. Open each one, go to the South West Water sheet and copy the rows whose site name contains *PORTHTOWAN* into a new workbook. Keep every column.
- [ ] **A4. Rainfall.** On the [Open-Meteo historical weather page](https://open-meteo.com/en/docs/historical-weather-api), set latitude `50.286`, longitude `-5.243`, start `2015-01-01`, end `2025-12-31`, daily variable *Precipitation sum*, timezone *Europe/London*, then download the CSV.

**Answer key for A3:** for 2022 to 2025 you should find Porthtowan No2 pumping station at 8, 27, 44 and 42 spills, and Porthtowan Beach No1 at 1, 5, 0 and 16 ([South West Water](https://www.southwestwater.co.uk/porthtowan)). If yours differ, that's a finding: note it.

---

## Part B: Build the workbook

Create `porthtowan_deep_dive.xlsx` in OneDrive with these tabs, in this order:

| Tab | What goes in it |
| --- | --- |
| `README` | Title, question, sources with links and download dates, tab list |
| `raw_samples` | A1 (and A2) pasted exactly as downloaded |
| `raw_spills` | A3 rows pasted exactly as downloaded, one block per year |
| `raw_rain` | A4 pasted exactly as downloaded |
| `clean_samples` | Cleaned sample table (Part C) |
| `clean_spills` | One tidy row per overflow per year (Part C) |
| `clean_rain` | Date and rainfall in mm |
| `dictionary` | Every column in the clean tabs: name, meaning, unit, source, rules applied |
| `analysis` | Pivot tables and calculations (Part D) |
| `dashboard` | One page of charts and slicers (Part E) |
| `notes` | Cleaning log, findings and limits (Part F) |

- [ ] Turn every clean range into an Excel table (**Insert > Table**) and name it: `tSamples`, `tSpills`, `tRain`. Named tables make every later formula readable.
- [ ] Never edit a `raw_` tab after pasting. All fixes happen in the clean tabs, so anyone can trace a number back to the source.

---

## Part C: Clean

Do each step in a new column next to the raw value, so the before and after sit side by side. Log every step in `notes` as *rule, column, rows affected*.

### Samples

- [ ] **C1. Dates.** Make sure sample dates are real dates, not text. If they're text, use `=DATEVALUE(TRIM(A2))`, or `=DATE(...)` with `TEXTSPLIT` if the format is unusual. Check with `=ISNUMBER(B2)`.
- [ ] **C2. Text tidy-up.** `=TRIM(CLEAN(A2))` on site names and any text column.
- [ ] **C3. Values like `<10` or `>10000`.** Lab results below or above what the test can measure are written with a symbol. Split them into two columns:
  - censored flag: `=IF(OR(LEFT(C2,1)="<",LEFT(C2,1)=">"),LEFT(C2,1),"")`
  - number: `=VALUE(SUBSTITUTE(SUBSTITUTE(C2,"<",""),">",""))`
  
  Write down your rule for using them (for example, "`<10` is treated as 10"), and why, in the dictionary.
- [ ] **C4. Filter to Porthtowan** (for the A2 archive): use `=FILTER(...)` on the site name or ID, or a filter and copy.
- [ ] **C5. Duplicates.** **Data > Remove Duplicates** on date and site. Record how many were removed.
- [ ] **C6. Helper columns:** `Year =YEAR([@Date])`, `Month =TEXT([@Date],"mmm")`, and `High =IF(OR([@Ecoli]>500,[@IE]>200),"High","OK")`.

> **About the "High" flag:** 500 E. coli and 200 intestinal enterococci per 100 ml are the coastal "Good" limits in the Bathing Water Regulations 2013. Those limits apply to a season's 95th percentile, not to one sample, so this flag is your own analysis threshold. Say so in the dictionary. Being clear about this is a sign of a careful analyst.

### Spills

- [ ] **C7. Reshape** the yearly blocks into one tidy table: `Year | Overflow | Spill count | Hours | Monitor uptime %`. Column names change between years, so map them by hand and note the mapping.
- [ ] **C8. Data validation.** Add a dropdown list on `Overflow` (**Data > Data Validation > List**) so only the two exact names can be used.
- [ ] **C9. Low uptime.** Flag any year where monitor uptime is under 90%: `=IF([@Uptime]<0.9,"Check","OK")`. A low count with low uptime isn't a clean year.

### Rain

- [ ] **C10.** Keep `Date` and `Rain_mm`. Add `Rain_3day` (that day plus the two before it): `=SUMIFS(tRain[Rain_mm],tRain[Date],">="&[@Date]-2,tRain[Date],"<="&[@Date])`.

📸 Screenshot one clean tab showing a raw value, its cleaned value and the flag.

---

## Part D: Analyse

- [ ] **D1. Join the rain to each sample** with XLOOKUP: `=XLOOKUP([@Date],tRain[Date],tRain[Rain_3day],"missing")`.
- [ ] **D2. Join each sample's year to that year's spill total:** `=SUMIFS(tSpills[Spill count],tSpills[Year],[@Year])`.
- [ ] **D3. Pivot 1, by year:** number of samples, average and maximum E. coli, count of "High", plus total spills.
- [ ] **D4. Pivot 2, wet and dry days:** group samples into `Rain_3day` of 0 mm, 0.1 to 5 mm and over 5 mm, then compare average E. coli and the share of "High" samples.
- [ ] **D5. Correlation:** `=CORREL(...)` between yearly spills and yearly share of "High" samples. With only a handful of years, write down why this is a hint, not proof.
- [ ] **D6. Case study, 7 September 2025:** find the rainfall for 4 to 7 September 2025, and the sample closest to that date. Did it show a "High" result? Did anything look unusual in the days before?

📸 Screenshot each pivot.

---

## Part E: Dashboard tab

- [ ] Year by year: spills as columns and the share of "High" samples as a line. If Excel for the web won't combine them, put two charts side by side with the same year axis.
- [ ] Wet against dry: a bar chart from Pivot 2.
- [ ] Rain and E. coli over time for the 2025 season, with 7 September marked.
- [ ] Add slicers for Year and Overflow (**PivotTable > Insert Slicer**).
- [ ] Three KPI cells across the top: total spills 2025, "High" samples 2025, change in spills since 2022.

📸 Screenshot the whole dashboard. This is your first LinkedIn post image.

---

## Part F: Write it up

- [ ] In `notes`, write three findings, one sentence each, each with its number.
- [ ] Write three limits, such as few samples per season, monitor uptime, and spill counts not showing volume.
- [ ] Write a sentence on what Excel couldn't do comfortably, such as the hourly timing of spills, England-wide data, or repeatable downloads. That sets up phase 2.
- [ ] Add a line to `AI_USAGE.md` for any help you got from AI in this phase.

---

## Part G: Publish

- [ ] Put the workbook and screenshots in this `excel/` folder (drag and drop on GitHub works, or tell me and I'll do it).
- [ ] Commit with the message `Phase 1: Excel deep dive`.
- [ ] Post on LinkedIn: your line about the Interclubs, the dashboard screenshot, one finding and a link to the repo.

## Skills you'll have shown

| IBM skill | Where |
| --- | --- |
| Importing and cleaning data | Part C, cleaning log |
| Text, date and lookup functions | C1 to C3, D1, D2 |
| Named tables and data validation | Part B, C8 |
| Pivot tables and slicers | D3, D4, Part E |
| Charts and a dashboard | Part E |
| Communicating findings and limits | Part F |
