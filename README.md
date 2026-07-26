# Car Sales Analysis Dashboard

## Project Overview
An end-to-end Excel data analysis project built on a real-world automotive sales dataset. The project covers the full analyst workflow: raw data audit, professional data cleaning, formula-driven analysis, and an interactive, presentation-ready dashboard — built entirely in Microsoft Excel.

## Objective
Turn a raw, imperfect car sales dataset into a clean, trustworthy analysis and a single-page executive dashboard that answers: which manufacturers and models sell best, how price/horsepower vary by vehicle type, and how price, horsepower, and sales relate to one another.

## Dataset Description
`Car_sales.csv` contains specification and sales data for 157 vehicle models from 30 manufacturers, including sales volume, price, resale value, engine/performance specs, physical dimensions, fuel economy, and launch date.

## Dataset Size
- **Rows:** 157
- **Columns:** 16
- **Duplicate rows:** 0 (verified with a full-row duplicate check)

## Data Cleaning
Cleaning was done deliberately, not by blanket mean-filling:
- The original file is preserved untouched in **Raw_Data**.
- A working copy was created in **Cleaned_Data**.
- Columns with only 1–3 missing values (Price, Engine_size, Horsepower, Wheelbase, Width, Length, Curb_weight, Fuel_capacity, Fuel_efficiency, Power_perf_factor) were imputed using the **median of the same Vehicle_type** (Car or Passenger) rather than a single global mean — this respects real differences between vehicle segments and is more outlier-resistant than a mean.
- **`__year_resale_value`** had 36 missing values (~23% of rows) — too many to safely impute without distorting the analysis. These were **left blank intentionally** and excluded from calculations; no KPI or chart in this project relies on that column.
- All numeric columns were converted to proper numeric (float) types.
- `Latest_Launch` was converted from text to a proper Excel date (`mm/dd/yyyy`).
- No rows were removed — all 157 original rows remain in Cleaned_Data.
- Every imputed cell is highlighted in yellow on the Cleaned_Data sheet and logged individually in **Data_Cleaning_Notes**.

## Missing Value Handling
| Column | Missing | Method |
|---|---|---|
| `__year_resale_value` | 36 | Left blank — excluded from analysis |
| `Price_in_thousands` | 2 | Median by Vehicle_type |
| `Engine_size` | 1 | Median by Vehicle_type |
| `Horsepower` | 1 | Median by Vehicle_type |
| `Wheelbase` | 1 | Median by Vehicle_type |
| `Width` | 1 | Median by Vehicle_type |
| `Length` | 1 | Median by Vehicle_type |
| `Curb_weight` | 2 | Median by Vehicle_type |
| `Fuel_capacity` | 1 | Median by Vehicle_type |
| `Fuel_efficiency` | 3 | Median by Vehicle_type |
| `Power_perf_factor` | 2 | Median by Vehicle_type |

Full row-level detail is in `documentation/data_cleaning_notes.md` and the `Data_Cleaning_Notes` sheet in the workbook.

## Duplicate Check
Checked all 16 columns for full-row duplicates using pandas `.duplicated()`. Result: **0 duplicates** — no rows removed.

## KPIs Created
1. **Total Sales** — sum of Sales_in_thousands = **8,320.7K units**
2. **Average Vehicle Price** — average of Price_in_thousands = **$27.34K**
3. **Average Horsepower** — average of Horsepower = **185.9 HP**
4. **Total Number of Models** — count of Model = **157**
5. **Total Number of Manufacturers** — count of unique Manufacturer = **30**

All KPIs are live Excel formulas (`SUM`, `AVERAGE`, `COUNTA`) referencing `Cleaned_Data`, not hardcoded numbers.

## Analysis Performed
Built on the `Analysis` sheet using `SUMIFS`/`AVERAGEIFS` formulas (recalculates automatically if the underlying data changes):
1. Total Sales by Manufacturer (all 30 manufacturers)
2. Average Price by Vehicle Type
3. Top 10 Best-Selling Car Models
4. Average Horsepower by Vehicle Type
5. Total Sales by Vehicle Type

## Dashboard Features
- Clean, single-page **Dashboard** sheet titled "CAR SALES ANALYSIS DASHBOARD"
- 5 KPI cards across the top
- 5 charts below: horizontal bar (sales by manufacturer), column chart (avg price by type), horizontal bar (top 10 models), column chart (avg horsepower by type), pie chart (sales share by type)
- 8 calculated insight statements
- Gridlines removed, consistent Arial font, professional blue/white color palette
- `AutoFilter` dropdowns on `Cleaned_Data` (Manufacturer, Vehicle_type) for interactive filtering. *Note: true clickable Slicers require a live Excel session on a native PivotTable — see the note on the Dashboard sheet for how to add them if you want that extra polish.*

## Key Insights
1. **Ford** is the top-selling manufacturer with **2,022.6K units** sold — more than double the #2 manufacturer (Dodge, 910.1K).
2. The **Ford F-Series** is the best-selling individual model at **540.6K units**.
3. "Passenger" vehicles carry a slightly higher average price (**$27.76K**) than "Car" vehicles (**$26.32K**).
4. Average horsepower is nearly identical between vehicle types — "Car" (186.4 HP) vs "Passenger" (185.8 HP), a gap of under 1 HP.
5. "Passenger" vehicles account for the larger share of total sales (**5,015.2K units, ~60.3%**) vs "Car" (3,305.5K, ~39.7%).
6. Price and Horsepower are strongly positively correlated (**r ≈ 0.84**) — higher-priced vehicles tend to be more powerful.
7. Neither Price nor Horsepower correlates positively with Sales (**r ≈ −0.30** and **r ≈ −0.20** respectively) — in this dataset, pricier/more powerful vehicles do not sell in higher volumes.
8. The top 5 manufacturers (Ford, Dodge, Toyota, Honda, Chevrolet) account for **~53.3%** of total industry sales in this dataset.

## Tools Used
- Microsoft Excel
- Excel formulas (`SUMIFS`, `AVERAGEIFS`, `SUM`, `AVERAGE`, `COUNTA`)
- Native Excel Charts (Bar, Column, Pie)
- AutoFilter (interactive filtering)
- Data cleaning & imputation methodology
- Dashboard design & data storytelling

## Project Structure
```
car-sales-excel-dashboard/
│
├── README.md
├── dataset/
│   └── Car_sales.csv
├── excel/
│   └── Car_Sales_Analysis_Dashboard.xlsx
├── images/
│   └── dashboard.png
└── documentation/
    └── data_cleaning_notes.md
```

## How to Use the Dashboard
1. Open `excel/Car_Sales_Analysis_Dashboard.xlsx` in Microsoft Excel.
2. Start on the **Dashboard** tab for the executive summary (KPIs, charts, insights).
3. Use the **AutoFilter** dropdowns on the **Cleaned_Data** sheet to filter by Manufacturer or Vehicle_type; the KPI cards and Analysis formulas will recalculate automatically since they reference this sheet.
4. Review **Analysis** for the underlying summary tables behind each chart.
5. Review **Data_Cleaning_Notes** for full documentation of every cleaning decision.
6. Review **Raw_Data** to see the dataset exactly as uploaded, unmodified.

---

## Resume Description

**Project Title:** Car Sales Analysis Dashboard — Excel Data Analytics Project

**Resume Bullet Points (ATS-friendly):**
- Built an end-to-end Excel dashboard analyzing 157 vehicle models across 30 manufacturers, applying a documented data-cleaning methodology (segment-based median imputation) to handle 51 missing values across 11 columns without discarding any records.
- Designed 5 KPI metrics and 5 dynamic PivotTable-style summary tables using SUMIFS/AVERAGEIFS formulas, translated into an interactive single-page dashboard with 5 native Excel charts and AutoFilter-based data exploration.
- Delivered 8 data-driven business insights (e.g., top-selling manufacturer/model, price-horsepower correlation of r=0.84) by analyzing sales, pricing, and performance relationships across vehicle segments.

**Technical Skills Demonstrated:**
- Microsoft Excel
- Data Cleaning & Imputation
- Excel Formulas: SUMIFS, AVERAGEIFS, SUM, AVERAGE, COUNTA
- Dashboard Design & Data Visualization
- Data Analysis & Insight Generation
- Correlation Analysis

