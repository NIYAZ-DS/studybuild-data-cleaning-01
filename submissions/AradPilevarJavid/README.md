# 📊 E-commerce Data Analysis

## ⚠️ Problem Statement
I’ve been given a real‑life dataset – real in the sense that it’s far from clean. It comes straight from an e‑commerce platform and, like most real‑world data, contains errors, missing values, and logical inconsistencies that need to be sorted out before any serious analysis can take place.

## 🎯 Goal
The aim of this project is to scrub the data until it’s reliable, consistent, and ready for either exploratory analysis or model training. Everything from fixing mislabelled genders to resolving impossible combinations of purchase counts and returns is fair game.

## 📁 Project Structure
```
AradPilevarJavid/
├── data/
│   └── cleaned_dataset.xlsx
├── notebook/
│   └── data_cleaning.ipynb # the full cleaning walkthrough
├── README.md
└── requirements.txt
```

🔒 **A note on ignored files:**  
Because this project is shared with others, I avoid a traditional `.gitignore` that could accidentally be committed or overridden. Instead, all local‑only files are listed inside `.git/info/exclude`. It works exactly like a `.gitignore` but lives in the repo’s internal Git folder, so it stays completely local and never interferes with collaborators.

## 🧹 What the Cleaning Covers
The notebook walks through each problem found in the dataset and how it was resolved. Here’s a quick overview of the main fixes:

- **Incorrect genders** – several male names were tagged as female. A list of known male names was used to reassign the `gender` column correctly.
- **Duplicate rows** – fully identical records were dropped.
- **Spelling mistakes** – the province `"Gilan"` was corrected to `"Guilan"`.
- **Outlier ages** – a value of `145` was almost certainly a typo for `45`, so it was replaced with the median.
- **Impossible return counts** – rows where `returned_items` exceeded `purchase_count` were fixed by swapping or adjusting the numbers to make logical sense.
- **Total spending mismatches** – some rows had a `total_spending` that didn’t match `avg_order_value × purchase_count`. Those were corrected after inspecting the raw numbers.
- **Missing values** – `NaN` entries in critical fields like `age` or `total_spending` were handled on a case‑by‑case basis.

No assumptions were made blindly – every change was checked against the surrounding data to keep it as faithful as possible to the original records.


---

📘 **For a complete description of every single decision, transformation, and sanity check, open the notebook – it contains the full, human‑written story of the cleaning process.**
