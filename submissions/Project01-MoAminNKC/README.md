# E-commerce Customer Data Cleaning Project (project-01-data-cleaning-MoAminNKC)

## Project Overview
In this project, I explored and cleaned a dataset of e-commerce customers to identify issues and logically correct them. The goal was to transform raw, messy data into a reliable dataset ready for Exploratory Data Analysis (EDA) and further business insights.

## Tools & Technologies
- **Languages/Libraries:** Python, Pandas, NumPy, ydata-profiling
- **Environments:** Jupyter Notebook
- **Software:** Microsoft Excel

## Data Cleaning Workflow

### 1. Handling Duplicates
- Identified and removed one completely duplicate row, reducing the dataset from 61 to 60 records.

### 2. Missing Values & Outliers
- **Age:** Addressed missing values and illogical entries (e.g., an age of 145, or ages < 0 / > 120) by replacing them with the median age (45) to prevent outliers from skewing the data.
- **Total Spending:** Resolved missing and incorrect values (e.g., a massive outlier of 25,000) by recalculating them using the established formula: `total_spending = purchase_count * avg_order_value`.
- **Handling Outliers:** To evaluate extreme high-spenders, I applied the Interquartile Range (IQR) method, which flagged 5 records exceeding the upper bound (`Q3 + 1.5 * IQR`). However, instead of capping or deleting these outliers, I retained them without modification. A manual review confirmed their values were logically sound (total spending perfectly aligned with the average order value multiplied by purchase count). Retaining them ensures the analysis accurately reflects our true, high-value VIP customers rather than treating them as data entry errors.

### 3. Structural Cleaning & Type Conversion
- Converted the `signup_date` column from string/object to proper datetime formats.
- Adjusted the `age` column to integer after resolving missing values.
- Stripped hidden leading/trailing spaces across all text columns.
- Standardized text casing (e.g., Title Case for names, cities, and membership tiers, and Upper Case for genders).

### 4. Logical Inconsistencies & Data Standardization
- **Purchase vs. Returns:** Discovered records where `returned_items` exceeded `purchase_count`. Rather than guessing, these impossible values were converted to NaN to maintain data integrity.
- **Gender Standardization:** I audited the logical relationship between customer first names and their recorded genders, discovering multiple inaccuracies. To establish a reliable ground truth, I imported a validated external name-to-gender dictionary to programmatically cross-reference and correct the invalid entries, ensuring high demographic accuracy.

### 5. Programmatic Validation & Flagging
- Advanced validation masks were created to programmatically flag categorical errors (invalid genders, tiers) and relational math mismatches.
- Rather than fixing these rows manually or silently deleting problematic data, I implemented a fully automated script-based approach to ensure reproducibility.
- For unresolvable data conflicts (such as impossible return counts), I utilized "missing indicator" (shadow) columns (e.g., `returned_items_is_missing`). This best-practice technique replaces the invalid data with `NaN` for clean math, while creating a permanent boolean flag to preserve the metadata of the data integrity failure.
- All corrections, imputations, and error flags were handled directly via Python, seamlessly generating the final `cleaned_dataset.xlsx` through a robust data pipeline.

### 6. Exploratory Data Analysis (EDA)
- With the dataset fully cleaned, column names were mapped to readable titles (e.g., `avg_order_value` to `Average Order Value`).
- Utilized `ydata_profiling` to automatically generate a comprehensive HTML report for in-depth data profiling. 👉 **[Click here to view the Customer Analysis Report](https://MoAminNKC.github.io/studybuild-data-cleaning-01/submissions/Project01-MoAminNKC/report/customer_analysis_report.html)**

## Final Result
The final output is a highly reliable and cleanly structured dataset of 60 records, free of duplicates, illogical values, and formatting errors. While 6 known missing values remain in the `returned_items` column, the dataset is fully prepared for business intelligence visualization and foundational analysis, alongside an automated profiling report for immediate business insights.