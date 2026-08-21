# Project 01 – Data Cleaning on an E-Commerce Dataset

## Project Overview

This project focuses on cleaning and improving the quality of an e-commerce dataset through a structured data cleaning workflow. The process involved inspecting the dataset, assessing data quality, handling missing and duplicate records, inspecting categorical values, correcting inconsistencies and data types, and investigating potentially unusual values.

The goal was to produce a clean, consistent, and reliable dataset that is ready for further analysis.

## Dataset Overview

The dataset contains **61 records** and multiple features related to customer purchasing behavior. The dataset includes information such as `customer_id`, `purchase_count`, `avg_order_value`, and `total_spending`.

Before the cleaning process, the dataset was examined to understand its structure, data types, missing values, duplicate records, categorical values, and potentially unusual entries.

## Data Cleaning Process

The following steps were performed during the cleaning process:

* Loaded and inspected the raw dataset
* Examined the dataset structure and features
* Assessed data quality and missing values
* Identified and handled duplicate records
* Inspected categorical values for inconsistencies
* Standardized and corrected inconsistent values
* Corrected inappropriate data types
* Investigated potentially unusual or invalid values
* Identified an inconsistency between `total_spending`, `purchase_count`, and `avg_order_value`
* Corrected the inconsistent `total_spending` value for `customer_id` 1030
* Performed a final validation of the cleaned dataset

## Tools & Libraries

* Python
* Pandas
* Matplotlib
* Visual Studio Code

## Project Structure

```text
project-01-data-cleaning-fatemeyousefia/
├── README.md
├── data/
│   ├── FirstDataset.xlsx
│   └── cleaned_dataset_fatemeyousefia.csv
└── notebook/
    └── data_cleaning_fatemeyousefia.ipynb
```

## Key Result

The cleaning process improved the consistency and reliability of the dataset by addressing the identified data quality issues and validating the final dataset.

One notable inconsistency was identified for `customer_id` 1030, where `total_spending` did not match the relationship between `purchase_count` and `avg_order_value`. The value was corrected based on the identified relationship and its consistency with the rest of the dataset.

## How to Run

1. Open `notebook/data_cleaning_fatemeyousefia.ipynb`.
2. Make sure Python and the required libraries are installed.
3. Run the notebook from start to finish.

The raw and cleaned datasets are stored in the `data/` directory, and the notebook uses relative paths to access the raw dataset.

## Conclusion

This project demonstrates a structured approach to identifying and addressing common data quality issues in an e-commerce dataset. The final cleaned dataset provides a more reliable foundation for subsequent data analysis.

