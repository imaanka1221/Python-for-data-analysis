# Day 2 – Electricity Consumption: Data Selection, Indexing & Sorting

## Overview
This notebook (`Day2.ipynb`) is the second stage of an electricity consumption data analysis project. It builds on Day 1's initial exploration and introduces core pandas techniques for **selecting columns, indexing rows, filtering with conditions, and sorting**.

## Environment
- **Platform**: Google Colab
- **Library**: `pandas`
- **Data source**: `/content/drive/MyDrive/Colab Notebooks/electricity consumption2.xlsx` (mounted via Google Drive)

## Notebook Steps

| Cell | Code | Purpose |
|---|---|---|
| Import library | `import pandas as pd` | Load the pandas library. |
| Load Dataset | `data = pd.read_excel(...)` | Load the dataset from Google Drive. |
| 1 | `data["Area"]` | Select a single column (`Area`) as a Series (300 rows). |
| 2 | `data[["Area", "Units_kWh", "Payment_Status"]]` | Select multiple columns at once. |
| 3 | `data.iloc[299]` | Select a single row by integer position (the last row, index 299). |
| 4 | `data.loc[0:10]` | Select a range of rows by label (rows 0–10, all columns). |
| 5 | `data.loc[10:20, ['Area', 'Payment_Status']]` | Select a range of rows AND specific columns by label. |
| 6 | `data.iloc[0:10]` | Select the first 10 rows by integer position. |
| 7 | `data.iloc[10:21, 1:4]` | Select rows and columns by integer position (rows 10–20, columns 1–3: `Reading_Date`, `Area`, `Customer_Type`). |
| 8 | `data[data["Units_kWh"] > 1000]` | Filter rows where consumption exceeds 1000 kWh (75 matching rows). |
| 9 | `data[data["Area"] == "Mogadishu"]` | Filter rows where `Area` is `Mogadishu` (98 matching rows). |
| 10 | `data[(data["Area"] == "Mogadishu") & (data["Units_kWh"] > 1000)]` | Combine multiple conditions with `&` (29 matching rows). |
| 11 | `data.sort_values('Area')` | Sort the entire dataset alphabetically by `Area`. |

## Key pandas Concepts Demonstrated
- **Column selection**: single column (`data["col"]`) vs. multiple columns (`data[["col1", "col2"]]`).
- **Label-based indexing** (`.loc`): select rows/columns by their labels; end of range is inclusive.
- **Position-based indexing** (`.iloc`): select rows/columns by integer position; end of range is exclusive.
- **Boolean filtering**: filter rows using a single condition or multiple combined conditions (`&` for AND).
- **Sorting**: reorder rows based on the values of a chosen column with `sort_values()`.

## Observations from the Data
- Filtering by `Area == "Mogadishu"` returns 98 rows, matching the earlier finding that `Mogadishu` is the most frequent area.
- Some rows contain the placeholder value `non_customer` in `Customer_Type` even within the `Mogadishu` filter results.
- `Payment_Status` values show inconsistent casing across the dataset (e.g., `Pending` vs. `pending`, `Overdue` vs. `OVERDUE`).
- Sorting by `Area` surfaces the placeholder value `non_area`, confirming it is a distinct (invalid) category alongside the real area names.

## Next Steps
- Clean inconsistent categories (`Payment_Status`, `Area`, `Customer_Type`) before further analysis.
- Use the filtering and sorting techniques shown here to isolate and inspect problematic rows for cleaning.
- Move on to grouping/aggregation for deeper insights (e.g., revenue by area, consumption trends over time).
