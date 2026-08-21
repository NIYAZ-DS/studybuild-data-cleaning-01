# 🛒 Customer Data Cleaning & Analysis

A complete data cleaning and analysis project for an online store's customer dataset — covering identification and resolution of data issues, handling missing values, removing duplicate records, and detecting outliers.

---

## 📌 Project Overview

This project performs a **full Data Cleaning pipeline** on a customer dataset. The data is first read from an Excel file, then processed using pandas, and finally prepared for statistical analysis.

---

## 🧰 Libraries Used

| Library | Purpose |
|---------|---------|
| `pandas` | Reading, processing, and manipulating data |
| `numpy` | Working with numeric values and NaN |
| `matplotlib` | Creating plots |
| `seaborn` | Statistical visualizations (histogram, boxplot) |

---

## 📊 Data Structure

- **Number of records:** 61
- **Number of columns:** 17
- **Data types:** Numeric (int/float), Text (object), Date (datetime)

### Main Columns:

| Column | Description |
|--------|-------------|
| `customer_id` | Customer identifier |
| `age` | Customer age |
| `gender` | Gender |
| `total_spending` | Total amount spent |
| `purchase_count` | Number of purchases |
| `avg_order_value` | Average value per order |
| `signup_date` | Membership signup date |

---

## 🔍 Issues Identified in the Data

### 1. Missing Values

Two columns contained missing values:

| Column | Missing Count | Handling Method |
|--------|--------------|-----------------|
| `age` | 1 | Replaced with the **Median** |
| `total_spending` | 1 | Computed from `purchase_count × avg_order_value` |

**Why the median?** Because the median is more robust to outliers than the mean, and it distorts the age distribution less.

**Why multiply the two columns?** Because `total_spending` logically equals the number of purchases times the average order value, so this method provides the most accurate estimate.

---

### 2. Duplicate Records

- **Number of duplicate rows:** 1
- Row **60** was an exact duplicate of row 13 (customer "Reza" with `customer_id = -10`)
- **Solution:** Removed the duplicate row using `df.drop(index=60)`

---

### 3. Outliers

Detected using the **IQR (Interquartile Range)** method:

```python
Q1 = df[column].quantile(0.25)
Q3 = df[column].quantile(0.75)
IQR = Q3 - Q1
lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR
```

**Results:**

| Column | Outlier Count | Indices |
|--------|--------------|---------|
| `age` | 1 | Row 11 (value 145, which was illogical) |
| `total_spending` | 5 | Rows 8, 11, 29, 43, 56 |

**Important note:** All `total_spending` values are **reasonable and valid** — they are only flagged as outliers due to the skewed distribution, not because they are errors. Therefore, they were **not removed**.

---

### 4. Logical Errors

| Issue | Solution |
|-------|----------|
| Some customers' gender was incorrect | Fixed using the `true_gender` dictionary based on first name |
| Row 23: `total_spending = 0` with `purchase_count = 0` | Set `avg_order_value = 0` for consistency |

---

## 🔧 Changes Applied

### 1. Data Type Optimization

```python
df['signup_date'] = pd.to_datetime(df['signup_date'])   # Convert to datetime
df['age'] = df['age'].astype('Int8')                     # Reduce memory usage
df['discount_used'] = df['discount_used'].map({'Yes': True, 'No': False}).astype('boolean')
```

Categorical columns (`gender`, `city`, `province`, ...) were converted to **category** type to reduce memory usage and improve processing speed.

### 2. Handling the Age Outlier

Row 11, which had an illogical age value (145), was first converted to `NaN` and then replaced with the median:

```python
df.loc[11, 'age'] = np.nan
df['age'] = df['age'].fillna(df['age'].median())
```

---

## 📈 Analyses Performed

### Age Histogram (Before & After Correction)

- **Before:** The age distribution had an unusual spike (outlier)
- **After:** More normal distribution with no outlier

### Boxplot of All Data

Used to visually display outliers across all numeric columns.

---

## ✅ Final Version vs. Initial Version

| Feature | Before | After |
|---------|--------|-------|
| Missing values | 2 | **0** |
| Duplicate rows | 1 | **0** |
| Data types | Default | **Optimized** (category, boolean, datetime) |
| Age outlier | 1 (illogical) | **Removed & replaced** |
| Logical errors | Several | **Fixed** |
| Memory usage | 8.2 KB | **5.7 KB** (30% less) |

---

## 🎯 Summary

This project implements a **complete data cleaning pipeline** that includes:

1. ✅ Reading data from multiple sources (Excel → CSV)
2. ✅ Inspecting data structure and quality
3. ✅ Handling missing values with logical methods
4. ✅ Removing duplicate records
5. ✅ Detecting and managing outliers
6. ✅ Fixing logical errors
7. ✅ Optimizing data types

The final result is a **clean, consistent dataset ready for modeling**.
