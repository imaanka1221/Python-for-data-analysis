# Electricity Consumption – Column Renaming, Value Counts & Type Conversion (with Outputs)

## Overview
These screenshots show a pandas data-cleaning workflow on the electricity consumption dataset, including the actual outputs/errors produced — which reveal exactly why some cleaning steps succeeded and others failed.

## Environment
- **Platform**: Google Colab
- **Library**: `pandas`
- **Data source**: `/content/drive/MyDrive/Colab Notebooks/my project4.xlsx`

## Steps & Outputs

| Step | Code | Output / Result |
|---|---|---|
| 1 | `data = pd.read_excel(...)` then `data["Area"]` | Loads the dataset; `Area` column has 300 rows. |
| 2 | `data.rename(columns={"Meter_ID": "MTR"})` | Preview only (not reassigned to `data`). Reveals `NaN` values in `Units_kWh` and `Customer_Type`, and an `Unknown` category in `Customer_Type`. |
| 3 | `data['Area'].value_counts()` | 8 unique values instead of 6 (see Data Quality Findings below). |
| 4 | `data['Payment_Status'].value_counts()` | 7 unique values instead of 3. |
| 5 | `data.rename(columns={"Area": "Location"})` | Preview only (not reassigned to `data`). |
| 6 | `data['Customer_Type'].value_counts()` | 6 unique values instead of 3. |
| 7 | `data['Customer_Type'].value_counts()` (re-run) | Same result — confirms no cleaning has occurred yet. |
| 8 | `data['Customer_Type'] = data['Customer_Type'].replace("residential", "Residential")` | Ran without error. |
| 9 | `data['Customer_Type'].value_counts()` | **Unchanged** — `residential` (1) still present. The `.replace()` call did not fix it. |
| 10 | `data['Customer_Type'] = data['Customer_Type'].replace("RESIDENTIAL ", "Residential")` (note trailing space) | Ran without error. |
| 11 | `data['Customer_Type'].value_counts()` | **Still unchanged** — `RESIDENTIAL` (1) and `residential` (1) remain. |
| 12 | `data["Tariff_per_kWh"] = data["Tariff_per_kWh"].astype(float)` | **Fails with `ValueError`** — the column contains values that cannot be directly cast to `float`. |
| 13 | `data["Tariff_per_kWh"] = pd.to_numeric(data["Tariff_per_kWh"], errors="coerce")` | Succeeds — invalid values are converted to `NaN` instead of raising an error. |
| 14 | `data['Customer_Type'].value_counts()` | Same 6-category result as before — `Tariff_per_kWh` cleanup doesn't affect `Customer_Type`. |

## Data Quality Findings (via `value_counts()`)

**`Area`** (8 unique values instead of 6):
`Mogadishu` (98), `Hargeisa` (57), `Bosaso` (38), `Garowe` (32), `Baidoa` (29), `Kismayo` (28), `baidoa` (2), `hargeisa` (1)

**`Payment_Status`** (7 unique values instead of 3):
`Paid` (187), `Pending` (51), `Overdue` (35), `pending` (3), `paid` (3), `Unknown` (1), `OVERDUE` (1)

**`Customer_Type`** (6 unique values instead of 3):
`Residential` (185), `Commercial` (75), `Industrial` (20), `Unknown` (2), `RESIDENTIAL` (1), `residential` (1)

All three categorical columns show the same root problem: **inconsistent capitalization** splits what should be one category into several.

## Why the `.replace()` Cleaning Attempts Failed
Two attempts were made to merge `residential` and `RESIDENTIAL` into `Residential`, and neither worked — the `value_counts()` results were identical before and after both calls. The likely explanation:
- `.replace("residential", "Residential")` requires an **exact** string match. If the actual cell value has extra whitespace or different casing than expected, it silently does nothing (no error is raised).
- `.replace("RESIDENTIAL ", "Residential")` includes a trailing space — but if the actual value's whitespace doesn't match exactly (e.g., a tab character, multiple spaces, or leading rather than trailing whitespace), the match still fails.

**Takeaway**: exact-match string replacement is fragile for real-world messy text. A more robust fix is to normalize case and whitespace first, e.g.:
```python
data['Customer_Type'] = data['Customer_Type'].str.strip().str.title()
```
This would consistently convert `residential`, `RESIDENTIAL`, and `Residential` into a single `Residential` category regardless of exact spacing or case.

## Why `astype(float)` Failed but `pd.to_numeric()` Worked
- `astype(float)` requires **every** value in the column to already be a valid numeric string; it raises a `ValueError` immediately if even one value can't be converted (e.g., text, blank, or malformed entries).
- `pd.to_numeric(..., errors="coerce")` is more forgiving: it converts what it can and replaces anything it can't parse with `NaN`, avoiding a crash. This is the standard approach for cleaning messy numeric columns, followed by inspecting/handling the resulting `NaN` values.

## Next Steps
- Normalize `Area`, `Payment_Status`, and `Customer_Type` using `.str.strip().str.title()` (or an explicit mapping) and re-verify with `value_counts()`.
- After running `pd.to_numeric()` on `Tariff_per_kWh`, check how many values became `NaN` and decide how to handle them (drop, impute, or investigate the source data).
- Persist column renames (`data = data.rename(...)`) if they are meant to be permanent.
