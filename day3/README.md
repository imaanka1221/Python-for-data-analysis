# Day 3 – Electricity Consumption: Data Cleaning (Renaming, Value Counts, Type Conversion)

## Overview
This notebook (`Day3.ipynb`) is the third stage of the electricity consumption data analysis project. It shifts from exploration/indexing (Days 1–2) into **data cleaning**: renaming columns, inspecting category frequencies with `value_counts()`, attempting to standardize inconsistent text values, and converting a column to a numeric type.

## Environment
- **Platform**: Google Colab
- **Library**: `pandas`
- **Data source**: `/content/drive/MyDrive/Colab Notebooks/my project4.xlsx` (a new/updated source file, different from the `electricity consumption2.xlsx` used in Days 1–2)

## Notebook Steps

| Cell | Code | Purpose |
|---|---|---|
| 1 | `import pandas as pd` | Load the pandas library. |
| 2 | `data = pd.read_excel(...)` | Load the dataset from Google Drive. |
| 3 | `data["Area"]` | Select the `Area` column. |
| 4 | `data.rename(columns={"Meter_ID": "MTR"})` | Rename `Meter_ID` to `MTR` (not saved back to `data`, so it's a preview only). |
| 5 | `data['Area'].value_counts()` | Count occurrences of each value in `Area`. |
| 6 | `data['Payment_Status'].value_counts()` | Count occurrences of each value in `Payment_Status`. |
| 7 | `data.rename(columns={"Area": "Location"})` | Rename `Area` to `Location` (also not saved back to `data`). |
| 8 | `data['Customer_Type'].value_counts()` | Count occurrences of each value in `Customer_Type`. |
| 9 | `data.dtypes` | Check column data types. |
| 10 | `data['Customer_Type'].value_counts()` | Re-check `Customer_Type` counts (unchanged). |
| 11 | `data['Customer_Type'] = data['Customer_Type'].replace("residential", "Residential")` | Attempt to standardize lowercase `"residential"` to `"Residential"`. |
| 12 | `data['Customer_Type'].value_counts()` | Re-check counts after the replace attempt. |
| 13 | `data['Customer_Type'] = data['Customer_Type'].replace("RESIDENTIAL\t", "Residential")` | Attempt to fix an uppercase value with a trailing tab character. |
| 14 | `data['Customer_Type'].value_counts()` | Re-check counts after the second replace attempt. |
| 15 | `data["Tariff_per_kWh"] = data["Tariff_per_kWh"].astype(float)` | Attempt to convert `Tariff_per_kWh` to a numeric (float) type. |
| 16 | `data["Tariff_per_kWh"] = pd.to_numeric(data["Tariff_per_kWh"], errors="coerce")` | Convert `Tariff_per_kWh` to numeric, turning unparseable values into `NaN`. |
| 17 | `data['Customer_Type'].value_counts()` | Final check of `Customer_Type` counts. |

## Data Quality Findings (via `value_counts()`)

**`Area`** (8 unique values instead of 6):
`Mogadishu` (98), `Hargeisa` (57), `Bosaso` (38), `Garowe` (32), `Baidoa` (29), `Kismayo` (28), `baidoa` (2), `hargeisa` (1)

**`Payment_Status`** (7 unique values instead of 3):
`Paid` (187), `Pending` (51), `Overdue` (35), `pending` (3), `paid` (3), `Unknown` (1), `OVERDUE` (1)

**`Customer_Type`** (6 unique values instead of 3):
`Residential` (185), `Commercial` (75), `Industrial` (20), `Unknown` (2), `RESIDENTIAL` (1), `residential` (1)

All three categorical columns show the same pattern: **inconsistent casing** creates duplicate categories that should really be one.

## Notable Issue: Cleaning Attempts Did Not Take Effect
The two `replace()` calls on `Customer_Type` (cells 11 and 13) did **not** change the value counts — `residential` and `RESIDENTIAL` still each appear once in the final count. Likely causes:
- The `.replace()` result wasn't reassigned correctly, or ran against stale data.
- The `"RESIDENTIAL\t"` replace target includes a trailing tab character that may not exactly match the actual value in the data (hidden/invisible whitespace mismatches are a common cause of failed string cleaning).

This is a good real-world lesson: **always verify a cleaning step worked** by re-running `value_counts()` afterward (which this notebook does do — it just doesn't yet fix the discrepancy).

## Other Observations
- `Meter_ID` → `MTR` and `Area` → `Location` renames were previewed but not persisted (the result of `.rename()` wasn't assigned back to `data`), so the original column names remain.
- `Tariff_per_kWh` was of type `object` (not numeric) in this dataset, unlike in the Day 1 dataset — likely due to non-numeric/malformed entries. `astype(float)` and `pd.to_numeric(..., errors="coerce")` were both used to attempt conversion; `to_numeric` with `errors="coerce"` is the safer approach since it turns invalid entries into `NaN` instead of raising an error.

## Next Steps
- Fix the `Customer_Type`, `Area`, and `Payment_Status` casing issues properly, e.g. with `.str.strip().str.title()` or an explicit mapping, and confirm with `value_counts()`.
- Persist the `rename()` results (`data = data.rename(...)`) if the renamed columns are meant to be kept.
- Check how many values were coerced to `NaN` in `Tariff_per_kWh` after `pd.to_numeric()`, and decide how to handle them.
