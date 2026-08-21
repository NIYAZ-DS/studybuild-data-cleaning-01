# Project 01: Customer Data Cleaning

## Project Overview

The objective of this project was to clean and prepare a customer dataset for further analysis. The dataset was examined for missing values, duplicate records, invalid values, inconsistent records, and incorrect data types to improve its overall quality and reliability.

---

## Dataset Issues Identified

During the data cleaning process, the following issues were identified:

- Missing values in the **age** and **total_spending** columns.
- Duplicate records.
- An unrealistic age value (145 years old).
- Inconsistent records where **purchase_count = 0** but **avg_order_value > 0**.
- Incorrect gender values in several records (educational dataset).
- Inconsistent **total_spending** values compared with `purchase_count × avg_order_value`.
- Six records where **returned_items > purchase_count**.

---

## Data Cleaning Actions

The following cleaning steps were performed:

- Removed duplicate records.
- Filled missing values in the **age** column using the median.
- Calculated missing **total_spending** values using:

  **purchase_count × avg_order_value**

- Calculated an `expected_total` value for all records using:

  **purchase_count × avg_order_value**

- Compared `expected_total` with `total_spending` and corrected inconsistent values, including Customer ID **1030**.
- Replaced the unrealistic age value (145 years) with the median age.
- Corrected incorrect gender values based on first names (for educational purposes).
- Flagged six records where **returned_items > purchase_count** without changing their original values.
- Converted the `signup_date` column to the **datetime** data type using Python.

---

## Why These Changes Were Made

- Duplicate records were removed to prevent duplicated observations.
- The median was used for the **age** column because it is less sensitive to extreme values.
- Missing **total_spending** values were calculated using existing business information rather than estimation.
- `expected_total` was calculated for all records to identify inconsistencies between purchase count, average order value, and total spending.
- Invalid age values were corrected because they were not realistic.
- Gender inconsistencies were corrected because this is an educational dataset and the names provided enough context for correction.
- Records with **returned_items > purchase_count** were flagged rather than changed because the correct values could not be determined with certainty.
- `signup_date` was converted to datetime in Python to ensure the column had the correct data type for further analysis.

---

## Missing Value Handling

Two missing values were identified:

- **age** → Replaced with the median.
- **total_spending** → Calculated using:

  **purchase_count × avg_order_value**

After the cleaning process, the dataset was checked again to ensure that missing values had been handled.

---

## Duplicate Handling

Duplicate records were identified using:

```python
df.duplicated()
```

After verifying the duplicated records, they were removed using:

```python
df.drop_duplicates()
```

---

## Data Type Changes

The `signup_date` column was converted to the `datetime` data type in Python:

```python
df["signup_date"] = pd.to_datetime(df["signup_date"])
```

This allows the date column to be used correctly in future date-based analysis.

---

## Outliers and Invalid Values

One unrealistic age value was detected:

- **Age = 145**

This value was replaced with the median age.

An inconsistent record was also identified where:

- `purchase_count = 0`
- `avg_order_value > 0`

Since the correct value could not be verified, the record was retained and documented rather than changed.

---

## Total Spending Validation

To validate the `total_spending` column, an expected total was calculated for every record:

```python
expected_total = purchase_count × avg_order_value
```

The calculated value was compared with the existing `total_spending` value. This process identified inconsistent records, including Customer ID **1030**, whose `total_spending` value was corrected based on the calculated expected total.

---

## Returned Items Validation

Six records were identified where:

```text
returned_items > purchase_count
```

Because the correct values could not be determined from the available data, these records were **flagged for review rather than modified**.

---

## Exploratory Data Analysis

After completing the final data cleaning and validation process, exploratory data analysis was performed to identify customer patterns and business insights.

### Age Distribution

The age distribution shows that customers are spread across a broad age range, with a higher concentration around the 55–60 age group.

### Membership Tier Distribution

The majority of customers belong to the Bronze membership tier, while fewer customers belong to the higher membership tiers.

### Average Spending by Membership

Silver members have the highest average spending among the membership tiers. After correcting the inconsistent `total_spending` value for Customer ID 1030, the overall pattern remained unchanged.

### Device Usage

Android is the most commonly used platform, followed by Web and iPhone. This indicates that Android represents the largest share of customer device usage.

---

## Business Insights

- Silver members show the highest average spending among membership tiers and could be prioritized for targeted retention campaigns.
- Android users represent the largest device segment, making mobile optimization particularly important.
- The concentration of customers around older age groups can be considered when designing targeted marketing campaigns.
- The large number of Bronze members suggests an opportunity to encourage customers to move toward higher membership tiers through loyalty programs and personalized offers.

---

## Final Dataset Improvements

Compared to the original dataset, the final version:

- Contains no duplicate records.
- Has missing values handled.
- Has unrealistic age values corrected.
- Has inconsistent `total_spending` values validated and corrected.
- Contains flagged records for `returned_items > purchase_count` without making unsupported assumptions.
- Has `signup_date` converted to datetime in Python.
- Has incorrect gender values corrected for educational purposes.
- Is ready for further exploratory analysis, dashboard development, and business analytics.

---

## Tools and Libraries

- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook
- Microsoft Excel
