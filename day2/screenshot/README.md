# Electricity Consumption – Data Selection, Indexing & Filtering

## Overview
This notebook builds on the initial exploration of the electricity consumption dataset and demonstrates core pandas techniques for **selecting columns, indexing rows, and filtering data** with conditions.

## Environment
- **Platform**: Google Colab
- **Library**: `pandas`
- **Data source**: `/content/drive/MyDrive/Colab Notebooks/electricity consumption2.xlsx` (mounted via Google Drive)

## Notebook Steps

| Step | Code | Purpose |
|---|---|---|
| 1 | `import pandas as pd` | Load the pandas library. |
| 2 | `data = pd.read_excel(...)` | Load the dataset from Google Drive. |
| 3 | `data["Area"]` | Select a single column (`Area`) as a Series (300 rows). |
| 4 | `data[["Area", "Units_kWh", "Payment_Status"]]` | Select multiple columns at once. |
| 5 | `data.iloc[299]` | Select a single row by integer position (the last row, index 299). |
| 6 | `data.loc[0:10]` | Select a range of rows by label (rows 0–10, all columns). |
| 7 | `data.loc[10:20, ['Area', 'Payment_Status']]` | Select a range of rows AND specific columns by label. |
| 8 | `data.iloc[0:10]` | Select the first 10 rows by integer position. |
| 9 | `data.iloc[10:21, 1:4]` | Select rows and columns by integer position (rows 10–20, columns 1–3: `Reading_Date`, `Area`, `Customer_Type`). |
| 10 | `data[data["Units_kWh"] > 1000]` | Filter rows where consumption exceeds 1000 kWh (75 matching rows). |
| 11 | `data[data["Area"] == "Mogadishu"]` | Filter rows where `Area` is `Mogadishu` (98 matching rows). |
| 12 | `data[(data["Area"] == "Mogadishu") & (data["Units_kWh"] > 1000)]` | Combine multiple conditions with `&` (29 matching rows). |

## Key pandas Concepts Demonstrated
- **Column selection**: single column (`data["col"]`) vs. multiple columns (`data[["col1", "col2"]]`).
- **Label-based indexing** (`.loc`): select rows/columns by their labels; end of range is inclusive.
- **Position-based indexing** (`.iloc`): select rows/columns by integer position; end of range is exclusive.
- **Boolean filtering**: filter rows using a condition (`data[condition]`) or multiple combined conditions (`&` for AND).

## Observations from the Data
- Filtering by `Area == "Mogadishu"` returns 98 rows, matching the earlier finding that `Mogadishu` is the most frequent area.
- Some rows contain the placeholder value `non_customer` in `Customer_Type` even within the `Mogadishu` filter results, confirming a known data quality issue.
- `Payment_Status` values show inconsistent casing across the dataset (e.g., `Pending` vs. `pending`, `Overdue` vs. `OVERDUE`).
- A placeholder value `non_area` also appears in the `Area` column.

## Next Steps
- Clean inconsistent categories (`Payment_Status`, `Area`, `Customer_Type`) before further analysis.
- Use the filtering techniques shown here to isolate and inspect problematic rows for cleaning.
