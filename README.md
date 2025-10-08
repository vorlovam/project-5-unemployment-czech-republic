# Power BI — Unemployment in the Czech Republic

Interactive Power BI dashboards visualizing labour-market trends and regional unemployment in the Czech Republic.  


---

## Repository contents
- **Unemployment_in_Czech_Republic.pbix** — main Power BI file with visuals and DAX measures.
- **Economic_Activity_in_the_Czech_Republic.csv** — national time series (unemployment, employment, inactive share).
- **Regional_Unemployment_Share.csv** — regional unemployment metrics and shares.

---

## Report pages
1. **Trends over time** — long-run development of employment, unemployment, and inactivity since 1993.
2. **Regions – trends** — unemployment over time by region (chosen via slicer).
3. **Regional Comparison** — cross-section comparison of regions in a selected year.  
   - **Key metrics:** *National Unemployment %*, *Selected Region Unemployment %*, *Gap to National (pp)*.
4. **Year slider controls** — the map on this page is filtered only by the slicer (not by a table).

---

## Core DAX measures

> **Formatting note:** Set **Display units = None** and **Decimal places = 2**  
> *(gaps are in percentage points, **pp**).*

```DAX
-- =========================================================
--  Base assumptions (rename columns/tables to match your model)
--  • 'Facts' table contains records with [Unemployment %]
--  • 'Region' is a dimension with [Region]
--  • 'Calendar' is a date table marked as Date table with [Date], [Year]
-- =========================================================

-- 1) National unemployment level (ignores Region context)
National Unemployment % :=
CALCULATE (
    AVERAGE ( Facts[Unemployment %] ),
    ALL ( 'Region'[Region] )
)

-- 2) Selected region’s unemployment (only when a single region is selected)
Selected Region Unemployment % :=
VAR _hasOne =
    HASONEVALUE ( 'Region'[Region] )
RETURN
IF (
    _hasOne,
    AVERAGE ( Facts[Unemployment %] ),
    BLANK ()
)

-- 3) Regional share of national (percentage points gap)
Gap to National (pp) :=
[Selected Region Unemployment %] - [National Unemployment %]

-- 4) Year picker helper (use on cards/charts that should follow the Year slicer)
Selected Year :=
SELECTEDVALUE ( 'Calendar'[Year] )

-- 5) Anchor for the map: keep map driven only by the Year slicer (ignore table visual filters)
--    Use this measure in the map’s tooltip or as a visual-level filter (is not blank)
Map Anchor (do not filter by table) :=
VAR _y = [Selected Year]
RETURN IF ( NOT ISBLANK ( _y ), 1 )

-- 6) Title helpers (optional)
Title — National vs Selected :=
VAR _r = SELECTEDVALUE ( 'Region'[Region], "—" )
VAR _y = [Selected Year]
RETURN
"Unemployment — National vs " & _r & " (" & _y & ")"
```

---

## How to use

1. Open **Unemployment_in_Czech_Republic.pbix** in **Microsoft Power BI Desktop (2024-05 or newer)**.  
2. On **Regional Comparison**, select a **Year** with the slicer to update the **map**, **bar chart**, and **cards**.  
3. On **Regions – trends**, pick a **Region** to follow its unemployment path over time.  
4. Ensure the measures above are added to your model and visuals.  
5. For *Gap to National (pp)* and other cards, set **Display units = None** and **Decimal places = 2**.

---

## Data sources & assumptions

- **Source:** Czech Statistical Office — Labour Market statistics (public data).  
- Rates are in **percent**; **gaps are in percentage points (pp)**.  
- Datasets were lightly cleaned (region names, simple calendar) for use with slicers.

---

## Tools

- Microsoft Power BI Desktop  
- Data shaping: **Power Query** and **DAX**

---

## Author

**Markéta Vorlová**
