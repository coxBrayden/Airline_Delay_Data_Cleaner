# Airline Delay Data Cleaning & Validation

This project focuses on **cleaning, validating, and preparing U.S. airline flight delay data** for analysis.  
The dataset contains over **400,000 records** detailing flight counts, delays, cancellations, and causes by airline and airport.  
The goal is to transform raw data into a clean, structured dataset ready for exploratory and statistical analysis.

---

## Overview

Before analyzing flight delays, it’s essential to ensure the underlying dataset is **complete, consistent, and reliable**.  
This notebook demonstrates a professional-grade **data cleaning and validation process**, including:

1. Profiling the raw dataset to detect missing values, duplicates, and outliers.  
2. Handling missing data intelligently (filling vs. dropping).  
3. Converting data types for correct numerical and datetime handling.  
4. Validating data integrity and logical consistency.  
5. Saving a cleaned version of the dataset for downstream analysis.

The end result is a reproducible, well-documented pipeline that makes any future analysis more accurate and efficient.

---

## Tools & Libraries

- **pandas** — data manipulation and cleaning  
- **NumPy** — numeric computation  
- **Matplotlib / Seaborn (optional)** — data profiling and visualization  
- **Regex (re)** — column name and text standardization  

---

## Dataset Description

Each record in the dataset represents monthly flight performance metrics by **airline** and **airport**.  
Key columns include:

| Column | Description |
|---------|--------------|
| `year`, `month` | Time of record |
| `carrier`, `carrier_name` | Airline identifier and name |
| `airport`, `airport_name` | Airport code and name |
| `arr_flights` | Total number of arriving flights |
| `arr_del15` | Number of delayed flights (15+ minutes) |
| `carrier_ct`, `weather_ct`, `nas_ct`, `security_ct`, `late_aircraft_ct` | Counts of delay causes |
| `arr_cancelled`, `arr_diverted` | Cancelled and diverted flights |
| `arr_delay`, `carrier_delay`, `weather_delay`, `nas_delay`, `security_delay`, `late_aircraft_delay` | Total delay times by cause |

---

## Cleaning Process

### 1. Data Profiling  
Initial profiling identifies the dataset shape, column types, missing values, and potential outliers.  
This helps determine what issues need to be addressed before cleaning.

### 2. Handling Missing Values  
A small number of numeric columns contained missing entries (<0.25% of rows).  
These were interpreted as **unreported values** (e.g., airports or months with no recorded flights) and replaced with `0` to maintain completeness.

### 3. Type Conversion  
Numeric-looking columns were converted to numeric types, and date fields (`year`, `month`) were combined into a single `date` column using the first day of each month.

### 4. Data Integrity Check  
Verified that no numeric values were negative and that no duplicates existed.  
All fields were validated for logical consistency (e.g., delay counts ≤ total flights).

### 5. Column Reordering & Export  
Columns were reordered for readability, with identifiers (date, carrier, airport) placed before metrics.  
The cleaned dataset was then saved as `Airline_Delay_Cause_Cleaned.csv`.

---

## Example Code Snippet

```python
# Fill missing numeric values with 0
numeric_cols = df.select_dtypes(include=[float, int]).columns
df[numeric_cols] = df[numeric_cols].fillna(0)

# Combine year and month into a single datetime column
df["date"] = pd.to_datetime(df[["year", "month"]].assign(day=1))

# Verify no negative or duplicate values
(df[numeric_cols] < 0).sum().sum()
df = df.drop_duplicates()

# Reorder columns for clarity
df = df[[
    "date", "year", "month", "carrier", "carrier_name", 
    "airport", "airport_name"
] + [c for c in df.columns if c not in [
    "date", "year", "month", "carrier", "carrier_name", 
    "airport", "airport_name"
]]]
# Export cleaned dataset
df.to_csv("Airline_Delay_Cause_Cleaned.csv", index=False)
```
---
## Example Output Summary
| Metric | Before Cleaning | After Cleaning|
|---------|--------------|------------------|
| `Rows` | 409,612| 409612|
| `Columns` | 21 | 22 (added `date`)|
| `Duplicates` | 0 | 0 |
| `Missing Values` | <0.25% | 0|
---
## Outcome
### After cleaning:
* The dataset is complete, consistent, and analysis-ready.
* No missing or duplicate records remain.
* All date, numeric, and categorical fields are properly typed and standardized.

This dataset is now suitable for visual or statistical analysis, such as identifying:

* Trends in flight delays over time.
* Which airlines or airports experience the highest delay rates.
* The relative impact of weather, carrier, and air traffic delays.
---
##Getting Started
###Installation
```bash
pip install pandas numpy matplotlib
```
###Usage
Open the `Airline_Delay_Data_Cleaner.ipynb` notebook.
Run all cells to profile, clean, and validate the dataset.
The cleaned dataset will be exported as `Airline_Delay_Cause_Cleaned.csv`.
