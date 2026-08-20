## Data Cleaning and Validation

### Missing Values and Invalid Data

First, the values in the different columns were examined. One **missing value (NaN)** was identified in the `age` column. In addition, one **invalid age value** was detected. The missing and invalid values were replaced using the **median age**. The median was chosen because it is generally more robust to outliers than the mean. The data type of the `age` column was also changed from `float` to `integer`.

In the `total_spending` column, one missing value (NaN) and one value inconsistent with the underlying calculation logic were identified. To validate this column, its values were compared with the result of `purchase_count × avg_order_value`, and the inconsistent values were corrected accordingly.

### Data Consistency Checks

Several values in the `gender` column were inconsistent with the gender associated with the corresponding names. To validate these values, the names in the dataset were compared with a reference dataset containing Iranian names and their associated genders. The records whose recorded gender differed from the reference were identified and corrected.

Initially, records where `purchase_count` was equal to zero while `returned_items` was greater than zero appeared to be inconsistent. However, after further investigation, it was determined that returned items were not included in the final purchase count. Therefore, cases such as `purchase_count = 0` and `returned_items > 0` were considered logically valid and were not treated as errors.

### Duplicate Records

Duplicate records were checked using `duplicated().sum()`. One duplicate record was identified and removed.

### Data Types

The data types of the columns were reviewed and modified where necessary to match the nature of the data. The `age` column was converted to `int64`, `signup_date` to `datetime64[ns]`, and `discount_used` to `bool`.

### Outlier Analysis

During the outlier analysis, one unusual value was identified in the `age` column and corrected. In addition, five outlier values were detected in `avg_order_value`. After investigation, these values were determined not to be data errors, so they were retained without modification.

### Tools

**Pandas** and **NumPy** were used for data inspection, validation, and cleaning.

### Final Dataset

Finally, the dataset was re-evaluated to ensure that the issues identified above had been addressed. The cleaned dataset was saved as `cleaned_dataset.xlsx`.
