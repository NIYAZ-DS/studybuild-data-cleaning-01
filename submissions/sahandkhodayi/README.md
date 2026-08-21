# Data Cleaning Report

## Overview

This project demonstrates a complete data cleaning workflow using **Python** and **Pandas**. The objective was to inspect the dataset, identify data quality issues, and perform the necessary preprocessing steps to produce a clean dataset suitable for further analysis or machine learning.

---

## Tools Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn

---

## Workflow

### 1. Data Loading

The dataset was imported from an Excel file using Pandas.

```python
pd.read_excel(...)
```

---

### 2. Initial Data Exploration

The following methods were used to understand the dataset:

* `df.info()`
* `df.describe()`
* `df.columns`
* `df.nunique()`
* `df.dtypes`

These provided information about:

* Number of rows and columns
* Data types
* Statistical summary
* Missing values
* Unique values

---

### 3. Missing Value Detection

Missing values were identified using:

```python
df.isna().sum()
```

The following missing values were found:

* `age`
* `total_spending`

---

### 4. Duplicate Detection and Removal

Duplicate records were identified using:

```python
df.duplicated()
```

After verifying the duplicated customer record, duplicates were removed with:

```python
df.drop_duplicates()
```

---

### 5. Date Formatting

The `signup_date` column was converted into the proper datetime format:

```python
pd.to_datetime(...)
```

---

### 6. Outlier Correction

An unrealistic age value of **145** was detected.

Since it was clearly an invalid entry, it was corrected to **45**.

```python
df.loc[df["age"] == 145, "age"] = 45
```

---

### 7. Missing Total Spending

A missing value in the `total_spending` column was recovered using the available information:

```
Total Spending = Quantity × Unit Price
```

The value was calculated instead of replacing it with a statistical estimate, preserving data accuracy.

---

### 8. Missing Age

The missing age value was replaced using the **median age** of the dataset.

```python
df["age"] = df["age"].fillna(df["age"].median())
```

Median was selected because it is less sensitive to outliers than the mean.

---

### 9. Data Type Conversion

After filling missing values, the `age` column was converted from float to integer.

```python
df["age"] = df["age"].astype(int)
```

---

## Final Validation

After cleaning, the dataset was verified using:

```python
df.info()
```

The final dataset contains:

* No duplicate records
* No missing values in the cleaned columns
* Correct data types
* Corrected invalid entries

---

## Summary of Cleaning Operations

* ✔ Explored dataset structure
* ✔ Inspected data types
* ✔ Identified missing values
* ✔ Removed duplicate records
* ✔ Converted date column to datetime
* ✔ Corrected an invalid age outlier
* ✔ Calculated missing total spending
* ✔ Filled missing age using the median
* ✔ Converted age to integer
* ✔ Validated the cleaned dataset

---

## Conclusion

The dataset has been successfully cleaned and prepared for future tasks such as:

* Exploratory Data Analysis (EDA)
* Statistical Analysis
* Machine Learning
* Data Visualization
* Predictive Modeling
