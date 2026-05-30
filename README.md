# 🧹 Project 1 — Data Cleaning
### DecodeLabs | Data Analytics Internship | Batch 2026

---

## 📌 Overview

This project covers the foundational phase of any data analytics pipeline: **cleaning raw data** before analysis or modelling begins. A dataset is only as reliable as the work that went into preparing it. This project identifies and resolves missing values, duplicate records, and incorrect data formats across an orders dataset.

---

## 🎯 Objectives

- Identify and handle **missing or null values**
- Detect and **remove duplicate rows**
- Correct **data formats** (dates, numbers, text, IDs)
- Validate **logical consistency** across related columns
- Export a clean, analysis-ready CSV file

---

## 📁 Project Files

| File | Description |
|---|---|
| `Dataset_for_Data_Analytics_-_Sheet1.csv` | Raw input dataset |
| `DataCleaning_Project1_DecodeLabs.ipynb` | Main Jupyter Notebook (fully executed) |
| `clean_dataset.py` | Original Python script (converted to notebook) |
| `cleaned_dataset.csv` | Output — cleaned and formatted dataset |
| `README_Project1_DataCleaning.md` | This file |

---

## 📊 Dataset

| Property | Value |
|---|---|
| Source | Orders Analytics Dataset |
| Raw Shape | 1,200 rows × 14 columns |
| Cleaned Shape | 1,200 rows × 14 columns |
| Date Range | Jan 2024 – Dec 2024 |

### Columns

| Column | Type (Raw) | Type (Cleaned) | Description |
|---|---|---|---|
| `OrderID` | object | string | Unique order identifier |
| `CustomerID` | object | string | Unique customer identifier |
| `Date` | object | datetime64 | Order date |
| `Product` | object | string (Title) | Product name |
| `Quantity` | int64 | int64 | Units ordered |
| `UnitPrice` | float64 | float64 | Price per unit ($) |
| `TotalPrice` | float64 | float64 | Total order value ($) |
| `PaymentMethod` | object | string (Title) | Payment type |
| `OrderStatus` | object | string (Title) | Current order status |
| `ReferralSource` | object | string (Title) | Where the customer came from |
| `CouponCode` | object | string (Title) | Discount code applied |
| `ItemsInCart` | int64 | int64 | Items browsed before purchase |
| `CustomerID` | object | string | Customer reference |
| `TrackingNumber` | object | string | Shipment tracking ID |

---

## 🔧 Cleaning Steps

### Step 1 — Load Raw Data
Load the CSV into a pandas DataFrame and print a shape/column report to understand the data before touching it.

### Step 2 — Identify Missing Values
Scan every column for nulls. Report the count and percentage per column.

**Finding:** `CouponCode` had **309 missing values (25.75%)**.  
**Fix:** Filled with `'NONE'` — indicating no coupon was applied to that order.

### Step 3 — Remove Duplicates
Check for fully duplicated rows and duplicate `OrderID` entries.

**Finding:** 0 full duplicate rows. 0 duplicate OrderIDs.  
**Fix:** `drop_duplicates()` applied as a safeguard for pipeline reuse.

### Step 4 — Correct Data Formats

| Sub-step | Action |
|---|---|
| 4a. Date | Converted `Date` from object → `datetime64` via `pd.to_datetime()` |
| 4b. Numerics | Coerced `Quantity`, `UnitPrice`, `TotalPrice`, `ItemsInCart` with `pd.to_numeric(errors='coerce')` |
| 4c. Text | Applied `.str.strip().str.title()` to all categorical text columns |
| 4d. IDs | Enforced `OrderID`, `CustomerID`, `TrackingNumber` as clean strings |

### Step 5 — Validate Logical Consistency
Verified that `TotalPrice == Quantity × UnitPrice` for all rows (tolerance ±$0.05).  
Also checked for non-positive values in `Quantity` and `UnitPrice`.

**Finding:** 0 mismatches. 0 negative/zero values. Data is logically consistent.

### Step 6 — Final dtype Check
Confirmed all columns carry the correct data type post-cleaning.

### Step 7 — Save Output
Exported the cleaned DataFrame to `cleaned_dataset.csv`.

---

## ✅ Cleaning Results Summary

| Check | Before | After |
|---|---|---|
| Missing values | 309 (`CouponCode`) | 0 |
| Duplicate rows | 0 | 0 |
| Duplicate OrderIDs | 0 | 0 |
| Date format | object (string) | datetime64 |
| Numeric coercion errors | 0 | 0 |
| TotalPrice mismatches | 0 | 0 |
| Negative/zero prices or qty | 0 | 0 |

---

## 🛠️ Tools & Libraries

| Tool | Purpose |
|---|---|
| Python 3.12 | Core language |
| pandas | Data loading, cleaning, export |
| numpy | Numeric operations |
| Jupyter Notebook | Interactive environment |

---

## ▶️ How to Run

1. Place `Dataset_for_Data_Analytics_-_Sheet1.csv` in the same folder as the notebook.
2. Open `DataCleaning_Project1_DecodeLabs.ipynb` in Jupyter.
3. Run all cells top-to-bottom (`Kernel → Restart & Run All`).
4. The cleaned file `cleaned_dataset.csv` will be saved in the same directory.

```bash
# Or run the original script directly
python clean_dataset.py
```

---

## 💡 Key Learnings

- Always profile your data **before** cleaning — understand null rates, types, and uniqueness first.
- Use **`pd.to_datetime(errors='coerce')`** and **`pd.to_numeric(errors='coerce')`** to safely convert without crashing on bad values.
- Filling `CouponCode` nulls with `'NONE'` is preferable to dropping rows — it preserves 25% of the dataset while remaining analytically honest.
- Logical consistency checks (e.g. `TotalPrice = Qty × UnitPrice`) catch calculation errors that type checks alone would miss.

---

*DecodeLabs | Batch 2026 | Project 1 of the Data Analytics Internship Track*
