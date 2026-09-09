# Electricity Consumption Dataset – Exploration Notebook Output

## Overview
These screenshots show the output of a pandas-based exploratory data analysis (EDA) notebook run on an electricity consumption dataset. The notebook loads the data and inspects its structure, data types, and summary statistics.

## Steps Shown

| Step | Code | Purpose |
|---|---|---|
| 1 | `data.head()` | Preview the first 5 rows of the dataset. |
| 2 | `data.tail()` | Preview the last 5 rows of the dataset. |
| 3 | `data.sample(5)` | View 5 randomly selected rows. |
| 4 | `data.shape` | Check the dataset's dimensions. |
| 5 | `data.columns` | List all column names. |
| 6 | `data.dtypes` | Check the data type of each column. |
| 7 | `data.info()` | Get a summary of non-null counts and dtypes. |
| 8 | `data.describe(include="all")` | Get summary statistics for every column. |

## Dataset Summary
- **Shape**: 300 rows × 8 columns
- **No missing values**: all 8 columns report 300 non-null entries

### Columns
| Column | Type | Notes |
|---|---|---|
| `Meter_ID` | object (text) | 300 unique values (one per row) |
| `Reading_Date` | datetime64 | Ranges from **2025-01-01** to **2026-08-01** |
| `Area` | object (text) | 9 unique values; `Mogadishu` is most frequent (98 rows) |
| `Customer_Type` | object (text) | 4 unique values; `Residential` is most frequent (187 rows) |
| `Units_kWh` | float64 | Mean ≈ 975 kWh; ranges from 0 to 10,022 kWh |
| `Tariff_per_kWh` | float64 | Mean ≈ 0.228; ranges from **-10** to 23 |
| `Payment_Status` | object (text) | 7 unique values (more than the expected Paid/Pending/Overdue, due to inconsistent casing); `Paid` is most frequent (187 rows) |
| `Total Revenue` | float64 | Mean ≈ 185.5; ranges from **-5,979** to 1,881.5 |

## Known Data Quality Issues
Based on the `describe(include="all")` output, the dataset has several issues that would need cleaning before analysis:
- **Negative values**: `Tariff_per_kWh` has a minimum of **-10**, and `Total Revenue` has a minimum of **-5,979** — both are invalid for real-world electricity billing and are likely data-entry errors.
- **Inconsistent text casing** in `Payment_Status`: 7 unique values exist instead of the expected 3 (e.g., `Paid`, `Pending`, `pending`, `non_pay`, `Overdue`).
- **Placeholder/invalid category values**: `non_customer` appears in `Customer_Type`, and similar placeholder values appear elsewhere in the dataset.
- **Zero-consumption records**: some rows have `Units_kWh = 0`, resulting in `Total Revenue = 0`.

## Next Steps
- Clean and standardize `Payment_Status` categories (fix casing, remove placeholders).
- Investigate and correct/remove negative `Tariff_per_kWh` and `Total Revenue` values.
- Handle or flag zero-consumption records.
- Proceed to deeper analysis (trends by area, customer type, and time).

