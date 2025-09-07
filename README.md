# Power BI – Unemployment in the Czech Republic

Interactive Power BI dashboards visualizing labor-market trends and regional unemployment in the Czech Republic.  
Prepared as the submission for **Project 5**.

## Repository contents
- **Unemployment_in_Czech_Republic.pbix** — main Power BI file with visuals and DAX measures  
- **Economic_Activity_in_the_Czech_Republic.csv** — national time series (employment, unemployment, inactive share)  
- **Regional_Unemployment_Share.csv** — regional unemployment metrics and shares

## Report pages
1. **Trends over time** — long-run development of employment, unemployment, and inactivity since 1990.  
2. **Regions – trends** — unemployment over time by region (region chosen via slicer).  
3. **Regional Comparison** — cross-section comparison of regions in a selected year.  
   - Key metrics: **National Unemployment %**, **Selected Region Unemployment %**, **Gap to National (pp)**  
   - A **Year slicer** controls the year; the map is **not** filtered by a table (per instructor’s note).

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
Formatting note: Use Display units = None and Decimal places = 2 (gaps are in percentage points, pp).

How to use

Open Unemployment_in_Czech_Republic.pbix in Microsoft Power BI Desktop (version 2024-05 or newer).

On Regional Comparison, select a year with the Year slicer to update the map, bar chart, and cards.

On Regions – trends, pick a region to follow its unemployment path over time.

Data sources & assumptions

Source: Czech Statistical Office — Labour Market statistics (public data).

Rates are in percent; gaps are in percentage points (pp).

Datasets were lightly cleaned (region names, simple calendar) for use with slicers.

Tools

Microsoft Power BI Desktop

Data shaping: Power Query and DAX

Author

Markéta Vorlová
