# Data Cleaning Notes — Car Sales Analysis

## 1. Dataset Overview
- Source file: `Car_sales.csv` (as uploaded)
- Rows: 157
- Columns: 16
- Duplicate rows found: 0

## 2. Duplicate Check
Checked full-row duplicates across all 16 columns using pandas `.duplicated()`.
**Result: 0 duplicate rows found.** No rows were removed on this basis.

## 3. Missing Value Summary (Raw Data)

| Column | Missing Count | % Missing | Handling Method |
|---|---|---|---|
| `__year_resale_value` | 36 | 22.9% | **Left blank (not imputed).** This many missing values (~23% of rows) is too large a share to safely impute with a mean/median without materially distorting resale-value analysis. Excluded from any calculation requiring full completeness. |
| `Price_in_thousands` | 2 | 1.3% | Imputed with the median `Price_in_thousands` of the same `Vehicle_type`. |
| `Engine_size` | 1 | 0.6% | Imputed with the median `Engine_size` of the same `Vehicle_type`. |
| `Horsepower` | 1 | 0.6% | Imputed with the median `Horsepower` of the same `Vehicle_type`. |
| `Wheelbase` | 1 | 0.6% | Imputed with the median `Wheelbase` of the same `Vehicle_type`. |
| `Width` | 1 | 0.6% | Imputed with the median `Width` of the same `Vehicle_type`. |
| `Length` | 1 | 0.6% | Imputed with the median `Length` of the same `Vehicle_type`. |
| `Curb_weight` | 2 | 1.3% | Imputed with the median `Curb_weight` of the same `Vehicle_type`. |
| `Fuel_capacity` | 1 | 0.6% | Imputed with the median `Fuel_capacity` of the same `Vehicle_type`. |
| `Fuel_efficiency` | 3 | 1.9% | Imputed with the median `Fuel_efficiency` of the same `Vehicle_type`. |
| `Power_perf_factor` | 2 | 1.3% | Imputed with the median `Power_perf_factor` of the same `Vehicle_type`. |

All other columns (`Manufacturer`, `Model`, `Sales_in_thousands`, `Vehicle_type`, `Latest_Launch`) had 0 missing values.

## 4. Why Median-by-Vehicle-Type Imputation?
For columns with only 1–3 missing values, the median of the same `Vehicle_type` (Car or Passenger) was used instead of a single global mean:
- **Preserves segment differences.** Vehicle segments differ meaningfully in weight, dimensions, and horsepower; a single dataset-wide average would blur those differences.
- **More robust to outliers than a mean**, since automotive specs (e.g., price, horsepower) can be right-skewed.
- **Low impact given the small counts.** Because each affected column is missing only 1–3 values out of 157, the choice of imputation method has negligible effect on aggregate KPIs and charts, while it keeps every row usable (no data loss).

## 5. Why `__year_resale_value` Was Handled Differently
With 36 of 157 rows missing (~23%), imputing this column — by mean, median, or any single-value method — risks materially distorting any analysis that relies on it, since nearly a quarter of the values would be invented rather than observed. Per data-cleaning best practice, the missing values were **left blank** rather than filled. No KPI, chart, or insight in this project depends on `__year_resale_value`; if a future analysis needs it, the recommended approach is to either (a) analyze only the 121 complete rows for that specific question, or (b) model the missingness explicitly rather than impute blindly.

## 6. Data Type Conversions
- All numeric columns (`Sales_in_thousands`, `__year_resale_value`, `Price_in_thousands`, `Engine_size`, `Horsepower`, `Wheelbase`, `Width`, `Length`, `Curb_weight`, `Fuel_capacity`, `Fuel_efficiency`, `Power_perf_factor`) were confirmed/converted to proper numeric (float) types.
- `Latest_Launch` was converted from text (e.g., `2/2/2012`) to a proper Excel date value, formatted as `mm/dd/yyyy`.

## 7. Row Integrity
No rows were deleted during cleaning. All 157 original rows are present in `Cleaned_Data`.

## 8. Full Imputed-Cell Log
The following 15 individual cell values were imputed. Each is also highlighted in yellow on the `Cleaned_Data` sheet in the workbook.

| Manufacturer | Model | Column | Vehicle Type | Imputed Value |
|---|---|---|---|---|
| Acura | CL | Price_in_thousands | Passenger | 22.695 |
| Chrysler | Town & Country | Price_in_thousands | Car | 24.072 |
| Chrysler | Town & Country | Engine_size | Car | 3.300 |
| Chrysler | Town & Country | Horsepower | Car | 185.000 |
| Chrysler | Town & Country | Wheelbase | Car | 112.200 |
| Chrysler | Town & Country | Width | Car | 71.950 |
| Chrysler | Town & Country | Length | Car | 191.600 |
| Cadillac | Seville | Curb_weight | Passenger | 3.197 |
| Chrysler | Town & Country | Curb_weight | Car | 3.890 |
| Chrysler | Town & Country | Fuel_capacity | Car | 20.700 |
| Chrysler | Town & Country | Fuel_efficiency | Car | 19.000 |
| Dodge | Intrepid | Fuel_efficiency | Passenger | 25.000 |
| Oldsmobile | Intrigue | Fuel_efficiency | Passenger | 25.000 |
| Acura | CL | Power_perf_factor | Passenger | 71.191 |
| Chrysler | Town & Country | Power_perf_factor | Car | 76.153 |

Note: the Chrysler Town & Country row had 7 of its 10 imputable fields missing — it was the most incomplete row in the raw dataset, but was still retained (not dropped) since its core identifying fields (Manufacturer, Model, Sales, Vehicle_type) were complete.
