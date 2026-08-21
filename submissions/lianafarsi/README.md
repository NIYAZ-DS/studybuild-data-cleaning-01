# Data Cleaning Project - First Dataset

## Project Overview
This repository contains the first data cleaning project for processing and standardizing customer data using Python (Pandas).

## Project Details & Process

**1. What problems did you find in the data?**
I found missing values (empty cells) in the 'age' and 'total_spending' columns. There were also completely duplicate rows, messy text with extra spaces, and dates that were not in a standard format.

**2. What changes did you make?**
I filled the missing values, deleted the duplicate rows, removed extra spaces from all text columns, and fixed the date formats.

**3. Why did you make those changes?**
Dirty data causes errors in analysis. I made these changes to make the dataset accurate, clean, and completely ready for the next steps of data analysis.

**4. How did you handle Missing Values?**
I did not delete the rows with missing data. Instead, I calculated the average (mean) of the 'age' and 'total_spending' columns and used `.fillna()` to put those averages in the empty cells.

**5. How did you check for Duplicates?**
I used the `df.duplicated().sum()` function to find and count them. After finding them, I used `df.drop_duplicates()` to remove the extra rows from the dataset.

**6. Did you change the data type of a column?**
Yes. I changed the 'signup_date' column into a standard time format (`datetime`). I also made sure text columns were formatted as strings before cleaning them.

**7. Did you find any outliers or illogical values?**
Yes, there were some invalid date entries. I used `errors='coerce'` in my code so that any illogical dates would safely turn into empty time values (`NaT`) without breaking the code. Text columns also had illogical extra spaces which I fixed using `.str.strip()`.

**8. What tools or libraries did you use?**
I used Python as the programming language and the Pandas library for data manipulation. The code was written and run in a Jupyter Notebook.

**9. What is the difference between the final version and the initial version?**
The initial version (`First Dataset.xlsx`) was messy, had errors, and included duplicate records. The final version (`cleaned_dataset_lianafarsi.xlsx`) is lighter, has no missing values, features standardized text and dates, and is 100% ready for analysis without the index column.

## Files Included
- `data_cleaning_lianafarsi.ipynb`: The Python script used for cleaning.
- `cleaned_dataset_lianafarsi.xlsx`: The final clean data.
- `README.md`: Project documentation.
