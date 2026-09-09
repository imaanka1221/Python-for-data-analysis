# Python-for-data-analysis

# Day 1 – Electricity Consumption: Initial Data Exploration

## Overview
This notebook (`Day1.ipynb`) performs the first-pass exploration ("Day 1" of a data analysis workflow) on an electricity consumption dataset. It loads the data from Google Drive (Google Colab environment) and runs a series of standard pandas checks to understand the dataset's structure, quality, and basic statistics.

## Environment
- **Platform**: Google Colab
- **Library**: `pandas`
- **Data source**: `/content/drive/MyDrive/Colab Notebooks/electricity consumption2.xlsx` (mounted via Google Drive)

## Notebook Steps
| Cell | Action | Purpose |
|---|---|---|
| 1 | `import pandas as pd` | Load the pandas library. |
| 2 | `pd.read_excel(...)` | Read the Excel file from Google Drive into a DataFrame. |
| 3 | `data.head()` | Preview the first 5 rows. |
| 4 | `data.tail()` | Preview the last 5 rows. |
| 5 | `data.sample(5)` | View 5 random rows. |
| 6 | `data.shape` | Check dataset dimensions. |
| 7 | `data.columns` | List column names. |
| 8 | `data.dtypes` | Check the data type of each column. |
| 9 | `data.info()` | Get a summary of columns, non-null counts, and memory usage. |
| 10 | `data.describe(include="all")` | Get summary statistics for all columns (numeric and categorical). |

## Dataset Summary
- **Shape**: 300 rows × 8 columns
- **No missing values**: all 8 columns have 300 non-null entries
- **Date range**: `Reading_Date` spans January 2025 to mid-2026

### Columns
| Column | Type | Notes |
|---|---|---|
| `Meter_ID` | object (text) | 300 unique meter IDs |
| `Reading_Date` | datetime64 | Date of the meter reading |
| `Area` | object (text) | 9 unique values (includes a `non_area` placeholder); `Mogadishu` is most frequent (98 rows) |
| `Customer_Type` | object (text) | 4 unique values (includes a `non_customer` placeholder); `Residential` is most frequent (187 rows) |
| `Units_kWh` | float64 | Electricity consumed; mean ≈ 975 kWh, min 0, includes some zero readings |
| `Tariff_per_kWh` | float64 | Price per kWh |
| `Payment_Status` | object (text) | Includes inconsistent casing (`Paid`/`Pending`/`pending`/`Overdue`) and a `non_pay` placeholder |
| `Total Revenue` | float64 | Calculated field (`Units_kWh × Tariff_per_kWh`) |

## Known Data Quality Issues
- Inconsistent text casing in `Payment_Status` (e.g., `Pending` vs. `pending`)
- Placeholder/invalid category values: `non_area`, `non_customer`, `non_pay`
- Some rows have `Units_kWh = 0`, resulting in `Total Revenue = 0`

## Next Steps
This notebook only covers initial exploration. Suggested follow-ups:
- Clean inconsistent categories and placeholder values
- Handle zero-consumption records
- Perform deeper analysis (trends by area, customer type, payment status, and time)

## Files
- `Day1.ipynb` — this notebook
- `electricity consumption2.xlsx` — source dataset (loaded from Google Drive, not included here)
