# Project 01 - Data Cleaning

## Overview

This project is the first project of the data science series. The main goal was to take a raw e-commerce customer dataset, check its quality, identify the problems inside it, clean the data, and prepare a reliable version for the next projects.

I tried to treat the dataset as a real dataset given to a data analyst. Because of that, I did not only look for missing values and duplicate rows. I also checked data types, invalid values, unusual values, relationships between columns, and some logical inconsistencies.

The main workflow was:

1. Load and understand the dataset
2. Check the structure and data types
3. Find missing values
4. Find duplicate records
5. Check categorical values
6. Check invalid numeric values
7. Check relationships between columns
8. Investigate unusual and inconsistent records
9. Apply the cleaning steps
10. Validate the cleaned dataset
11. Export the final dataset

## Dataset

The original dataset contains 61 rows and 17 columns.

The columns include:

- `customer_id`
- `first_name`
- `gender`
- `age`
- `city`
- `province`
- `signup_date`
- `membership_tier`
- `purchase_count`
- `avg_order_value`
- `total_spending`
- `last_purchase_days`
- `payment_method`
- `device`
- `discount_used`
- `returned_items`
- `satisfaction_score`

The dataset contains customer information as well as purchasing behavior and satisfaction information.

## Tools and Libraries

The project was completed in Python using Jupyter Notebook.

Main libraries:

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn

## Initial Data Inspection

The first checks were used to understand the size, structure, data types, and basic statistics of the raw data.

The original dataset had:

- 61 rows
- 17 columns
- 2 missing cells
- 1 duplicated row
- 1 duplicated `customer_id`

The `signup_date` column was initially stored as an object/string.

## Missing Values

Two missing values were found in the raw dataset.

### Missing `age`

One missing value was found for customer ID `1020`.

The missing age was handled after invalid age values were checked. The invalid age value `145` was first treated as missing, and then the missing age was filled using the median of the valid age values.

### Missing `total_spending`

One missing value was found for customer ID `1040`.

The dataset has a useful relationship between:

`purchase_count * avg_order_value`

and

`total_spending`.

Because of this relationship, the missing value was reconstructed instead of using a general average.

## Duplicate Records

One exact duplicate row was found.

The duplicated record belonged to customer ID `1014` and the two rows were completely identical.

Since it was an exact duplicate, one copy was removed.

After removing the duplicate, the dataset contained 60 rows.

## Data Type Check

The `signup_date` column was initially stored as text.

It was converted to the `datetime` data type using pandas.

The final data types of the checked columns were:

- `age` -> integer
- `purchase_count` -> integer
- `signup_date` -> datetime

This makes the dataset more suitable for later analysis.

## Invalid Age Value

One clearly invalid age was found:

- customer ID `1010`
- age = `145`

This value is outside the reasonable range used in this project.

It was replaced with a missing value and the missing value was then filled using the median age.

After cleaning:

- minimum age = `19`
- maximum age = `65`

## High `total_spending` Investigation

Very high values of `total_spending` were checked separately using:

```python
df[df["total_spending"] > 20000]
```

This check identified the unusually large value of `25000`.

However, the value was not changed only because it was large. It was compared with the relationship between `purchase_count`, `avg_order_value`, and `total_spending`.

For customer ID `1030`:

- `purchase_count` = 26
- `avg_order_value` = 156.89
- recorded `total_spending` = 25000
- expected `total_spending` = 4079.14

This showed that the value was inconsistent with the other columns, so it was reconstructed using the formula:

```text
purchase_count * avg_order_value
```

## Total Spending Consistency Check

The same relationship was checked for the whole dataset.

The formula used was:

```python
expected = df_clean["avg_order_value"] * df_clean["purchase_count"]
```

After cleaning, the maximum absolute difference between `total_spending` and the calculated value was:

`1.8189894035458565e-12`

This is effectively zero and comes from normal floating-point precision.

## `purchase_count = 0` Investigation

Customer ID `1024` had:

- `purchase_count` = 0
- `avg_order_value` = 63.67
- `returned_items` = 2
- `total_spending` = 0

This was treated as a logical inconsistency that required further investigation.

The value `0` for `purchase_count` was replaced using the median of the valid non-zero purchase counts.

The resulting median was `17`.

After this change, `total_spending` was recalculated using:

```text
purchase_count * avg_order_value
```

The customer therefore had:

- `purchase_count` = 17
- `total_spending` = 1082.39

## `first_name` and `gender` Consistency

The relationship between `first_name` and `gender` was also investigated because several names appeared with different gender values.

The original `gender` column was not overwritten.

Instead, a separate column called `gender_inferred` was created using a first-name mapping.

This was done to keep the original data available and clearly separate the inferred value from the original recorded value.

The final validation showed that `gender_inferred` contains no missing values.

## Other Numeric Checks

The dataset was also checked for negative or invalid values.

The following checks were performed:

- negative `purchase_count`
- negative `avg_order_value`
- negative `total_spending`
- negative `returned_items`
- invalid `satisfaction_score`

No invalid negative values were found in these checks, and the satisfaction scores remained within the expected range of 1 to 5.

## Returned Items and Purchase Count

The relationship between `returned_items` and `purchase_count` was checked.

Initially, 6 records had:

```text
returned_items > purchase_count
```

After the correction of customer ID `1024`, 5 records still had this condition:

- customer 1008
- customer 1015
- customer 1029
- customer 1037
- customer 1056

These values were not automatically changed.

The reason is that the available columns were not enough to prove that these records were actually wrong. `purchase_count` and `returned_items` may represent different units or business definitions.

Therefore, the issue was documented instead of making an unsupported correction.

## Categorical and Text Checks

The following categorical columns were checked:

- `gender`
- `city`
- `province`
- `membership_tier`
- `payment_method`
- `device`
- `discount_used`

Their values and frequencies were examined for obvious spelling, casing, or formatting problems.

Text columns were also checked for unnecessary spaces.

No extra spaces were found.

## City and Province Check

The relationship between `city` and `province` was checked to see whether one city was associated with more than one province.

No inconsistent city/province mapping was found in the dataset.

## Date Check

The `signup_date` column was converted from object to datetime.

The conversion was successful and no invalid dates remained.

## Outlier Detection

The IQR method was used to detect possible outliers.

The initial results were:

- `age`: 1 outlier
- `purchase_count`: 0 outliers
- `avg_order_value`: 0 outliers
- `total_spending`: 5 outliers
- `last_purchase_days`: 0 outliers
- `returned_items`: 0 outliers
- `satisfaction_score`: 0 outliers

Outlier detection was used as an investigation step.

An outlier was not automatically considered an error. For example, the high `total_spending` value of `25000` was corrected because it was also inconsistent with the internal formula of the dataset, not only because it was statistically unusual.

## Cleaning Steps Applied

The following changes were applied to create the cleaned dataset:

1. Removed the exact duplicated row for customer ID `1014`.
2. Replaced the invalid age value `145` with a missing value and filled it using the median valid age.
3. Reconstructed the missing `total_spending` value for customer ID `1040`.
4. Reconstructed the inconsistent `total_spending` value for customer ID `1030`.
5. Investigated customer ID `1024` and replaced its invalid `purchase_count = 0` with the median valid non-zero purchase count.
6. Recalculated `total_spending` for customer ID `1024`.
7. Converted `signup_date` to datetime.
8. Converted `age` and `purchase_count` to integer types after imputation.
9. Checked and standardized text spacing.
10. Added `gender_inferred` as a separate derived column.
11. Reset the dataframe index after cleaning.

## Final Validation

The final validation showed that the main cleaning steps were completed successfully.

### Missing values

```text
Total missing values: 0
```

### Duplicates

```text
Duplicate rows: 0
Duplicate customer IDs: 0
```

### Gender inference

```text
Missing inferred gender: 0
```

### Age range

```text
Minimum age: 19
Maximum age: 65
```

### Total spending consistency

```text
Maximum difference: 1.8189894035458565e-12
```

### Remaining logical inconsistency

After the customer 1024 correction, 5 records still had:

```text
returned_items > purchase_count
```

These records were retained and documented rather than changed without enough evidence.

## Final Dataset

After removing the duplicate row, the cleaned dataset contains 60 rows and 18 columns.

The additional column is:

- `gender_inferred`

The original `gender` column was kept unchanged.

## Visualizations

The notebook includes several visual checks:

- age distribution after cleaning
- total spending distribution after cleaning
- total spending before and after cleaning using a boxplot

These visualizations were used to support the cleaning and validation process.

## Output Files

The main project outputs are:

- `cleaned_dataset_malihehabbasi-ml.xlsx`
- `cleaned_dataset_malihehabbasi-ml.csv`
- `data_cleaning_malihehabbasi-ml.ipynb`

## Project Structure

```text
project-01-data-cleaning-malihehabbasi-ml/
|
|-- data/
|   |-- cleaned_dataset_malihehabbasi-ml.xlsx
|   `-- cleaned_dataset_malihehabbasi-ml.csv
|
|-- notebook/
|   `-- data_cleaning_malihehabbasi-ml.ipynb
|
`-- README.md
```

## Conclusion

This project was mainly about learning how to inspect a raw dataset before using it for analysis.

The cleaning process was not based on removing every unusual value. For each problem, the data was checked first and a correction was made only when there was enough evidence to support it.

The final dataset has no missing values, no duplicate rows, no duplicate customer IDs, valid age values, a correct date type, consistent `total_spending`, and a separate inferred gender column.

The five remaining `returned_items > purchase_count` cases were kept because there was not enough information to prove that they were data errors.

The cleaned dataset is now ready to be used in the next projects for exploratory data analysis, data visualization, customer behavior analysis, sales analysis, dashboards, and later machine learning work.

## Author

Maliheh Abbasi

## Date

August 2026