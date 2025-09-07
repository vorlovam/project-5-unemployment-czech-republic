# Power BI – Unemployment in the Czech Republic

Interactive Power BI dashboards visualizing labor-market trends and regional unemployment in the Czech Republic.  
Prepared as the submission for **Project 5**.

---

## Repository contents
- **Unemployment_in_Czech_Republic.pbix** – main Power BI file with visuals and DAX measures  
- **Economic_Activity_in_the_Czech_Republic.csv** – national time series (employment, unemployment, inactive share)  
- **Regional_Unemployment_Share.csv** – regional unemployment metrics and shares

---

## Report pages
1. **Trends over time** – long-run development of employment, unemployment and inactivity since 1990.  
2. **Regions – trends** – unemployment over time by region; region chosen via slicer.  
3. **Regional Comparison** – cross-section comparison of regions in a selected year.  
   - Key metrics: **National Unemployment %**, **Selected Region Unemployment %**, **Gap to National (pp)**.  
   - A **Year slicer** controls the year; the map is **not** filtered by the table (per instructor’s note).

---

## Core DAX measures
```DAX
-- National unemployment level
National Unemployment % =
AVERAGE('Economic_Activity_in_the_Czech_Republic'[Unemployment Rate (%)])

-- Regional level shown only when a single region is selected
Selected Region Unemployment % =
IF(
    HASONEVALUE('Regional_Unemployment_Share'[Region]),
    [Regional Unemployment %],
    BLANK()
)

-- Regional minus national (percentage points)
Gap to National (pp) =
[Regional Unemployment %] - [National Unemployment %]

-- Same gap, displayed only for a single selected region
Gap to National (pp) Selected =
IF(
    HASONEVALUE('Regional_Unemployment_Share'[Region]),
    [Gap to National (pp)],
    BLANK()
)
