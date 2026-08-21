# Project 01 - Data Cleaning on an E-commerce Dataset

## Overview

This project takes a raw customer dataset from an online store and cleans it into a standardized, analysis-ready version. The raw file (`raw_dataset.xlsx`) contains 61 customer records across 17 columns. Running `data_cleaning.ipynb` on this file produces `cleaned_dataset.xlsx` / `cleaned_dataset.csv` — a version with 0 missing values, 0 duplicate records, consistent data types, and no known logical inconsistencies left unresolved (except two flagged edge cases explained below, which were deliberately left untouched).

## Dataset

One row per customer. Columns: customer_id, first_name, gender, age, city, province, signup_date, membership_tier, purchase_count, avg_order_value, total_spending, last_purchase_days, payment_method, device, discount_used, returned_items, satisfaction_score.

-----

## What problems were found in the data?

|Problem                   |Details                                                                                                                                               |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
|Missing values            |1 missing in `age`, 1 missing in `total_spending`                                                                                                     |
|Duplicate record          |1 fully duplicated row (customer_id 1014)                                                                                                             |
|Incorrect data types      |IDs/counts not consistently integer, `discount_used` stored as Yes/No text instead of boolean                                                         |
|Invalid value             |1 unrealistic age (145, customer 1010)                                                                                                                |
|Outliers                  |2 cases where `total_spending` didn’t match `purchase_count × avg_order_value`                                                                        |
|Inconsistent naming/casing|text columns had mixed casing (e.g. “Vip” vs “VIP”, “iPhone” vs “Iphone”)                                                                             |
|Gender/name mismatch      |38 rows where the recorded gender didn’t match what the Persian first name implies                                                                    |
|Outdated province name    |“Khorasan” used instead of the current “Khorasan Razavi”                                                                                              |
|Logical inconsistency     |6 rows where `returned_items` was greater than `purchase_count`, which isn’t possible; 38 rows where `gender` didn’t match what the first name implies|

No typos, no extra whitespace, no unparseable or future dates were found in this dataset.

## What changes were made, and why?

- **Missing values** — filled instead of dropped, to keep the customer’s other valid fields. See dedicated section below.
- **Duplicate row** — removed, since a repeated identical row would double-count that customer in any later totals.
- **Data types** — enforced (int for IDs/counts, float for money, boolean for discount_used) because grouping and calculations later depend on consistent types.
- **Age = 145** — replaced with the median, since no human is 145 years old; almost certainly a typo.
- **total_spending outlier (customer 1030)** — the gap from the expected value was more than 5x, which strongly suggested an accidental extra digit, so it was corrected. A second, smaller mismatch (customer 1040) was flagged but left unchanged, since it wasn’t clear whether `total_spending`, `purchase_count`, or `avg_order_value` was the actual error — correcting it would have meant guessing.
- **Casing** — standardized to Title Case across all text columns so that, for example, a groupby on `membership_tier` doesn’t split “VIP” and “Vip” into two separate groups.
- **Gender vs. name** — corrected based on first name. Persian first names in this dataset are not shared between genders (unlike a name like “Jordan” in English), so a name appearing with both M and F genders is a sign the gender field was entered wrong, not the name.
- **Province name** — updated “Khorasan” to “Khorasan Razavi” for accuracy, but only after confirming every city consistently mapped to one province already (so this wasn’t a data inconsistency, just an outdated regional name — Iran split Khorasan into three provinces in 2004).
- **returned_items > purchase_count** — flagged, not corrected, for the same reason as the total_spending edge case: not enough information to know which column is actually wrong.

## How were missing values handled?

`df.isna().sum()` showed 2 missing values: one in `age`, one in `total_spending`. Both were numeric columns, so both were filled with the column median rather than the mean, since the median is less affected by outliers already present in the data. No rows were dropped — a customer with one missing value still has 16 other valid fields, and dropping the row would discard that information for no reason.

## How were duplicates checked?

Checked with `df.duplicated(subset=['customer_id'])`, using `customer_id` as the unique key rather than checking for fully identical rows across all columns, since customer_id is the field that should never repeat regardless of whether every other column happens to match. Found 1 duplicate (customer_id 1014). Kept the first occurrence, dropped the second, using `drop_duplicates(keep='first')`.

## Were column data types changed?

Yes:

- `customer_id`, `purchase_count`, `last_purchase_days`, `returned_items`, `satisfaction_score` → converted to `int`
- `avg_order_value`, `total_spending` → converted to `float`, rounded to 2 decimals
- `discount_used` → converted from text (“Yes”/“No”) to boolean (`True`/`False`)
- `signup_date` → converted from text to a proper `datetime` type

## Were outliers or illogical values found?

Yes, three categories:

1. **Outliers in `total_spending`** — compared against `purchase_count × avg_order_value`. One case (customer 1030) was corrected as an obvious data entry error; one case (customer 1040) was flagged and left as-is.
2. **Logical inconsistency between returned_items and purchase_count** — 6 rows where `returned_items` exceeded `purchase_count`, which is not physically possible. Flagged, not corrected, since it’s unclear which of the two columns is wrong.
3. **Logical inconsistency between gender and first_name** — 38 rows where the recorded gender contradicted what the Persian first name implies (each name in this dataset is exclusively male or exclusively female). This one was corrected, since the fix was based on strong evidence rather than a guess — the name itself is a reliable signal, so gender was updated to match it.

Also checked and found clean: no negative values in purchase_count, avg_order_value, total_spending, or returned_items; no satisfaction_score outside the 1–5 range; no customers with purchase_count = 0 but total_spending > 0.

## What tools/libraries were used?

Python, pandas, numpy, openpyxl.

## What’s different between the final version and the original?

|                               |Raw                  |Cleaned                           |
|-------------------------------|---------------------|----------------------------------|
|Rows                           |61                   |60                                |
|Missing values                 |2                    |0                                 |
|Duplicate customer_id          |1                    |0                                 |
|Unrealistic age values         |1                    |0                                 |
|total_spending outliers        |2                    |1 corrected, 1 flagged (unchanged)|
|Gender/name mismatches         |38                   |0                                 |
|returned_items > purchase_count|6                    |6 (flagged, unchanged)            |
|Data types                     |mixed                |consistent, enforced              |
|Text casing                    |inconsistent         |standardized (Title Case)         |
|Province naming                |outdated (“Khorasan”)|current (“Khorasan Razavi”)       |

## Limitations

- Several decisions here are judgment calls (e.g. the 5x threshold used to decide whether an outlier was “obviously” a data entry error) that would normally be confirmed with the data owner in a real work setting rather than resolved unilaterally.
- Filling missing values with the median assumes the missingness is random. If it isn’t, this could introduce a small bias.
- The gender correction relies on a fixed list of names found in this specific dataset. A different file with different names would need that list updated.
- Two known issues (the customer 1040 spending mismatch, and the 6 rows with returned_items > purchase_count) were deliberately left uncorrected rather than guessed at, and should be reviewed manually if a fully “clean” answer is required.

## How to Run

1. Place `raw_dataset.xlsx` in the same folder as `data_cleaning.ipynb`.
2. Open in Jupyter, VS Code, or Google Colab.
3. Run all cells top to bottom.
4. Review the printed output at each step — that’s where the findings are shown.
5. The notebook saves `cleaned_dataset.xlsx` and `cleaned_dataset.csv` at the end.

-----

## Questions This Cleaned Dataset Should Be Able to Answer

- **Which cities generate the most revenue?** Mashhad, followed by Tabriz — see the notebook for the full ranking by `total_spending`.
- **Which customers bought the most?** Costomer IDs: 1006, 1040, 1009, 1044, 1023.
- **What’s the average order value?** 213.1565
- **Which payment method is used most?** Online Wallet.
- **What effect do discounts have on spending?** CDiscount use didn’t increase how often customers bought (purchase count is nearly identical: 17.5 vs 17.3). But customers who used a discount spent less per order (203 vs 221) and less in total (2,917 vs 3,737). The one place discounts showed a real benefit was satisfaction — discount users were noticeably happier on average (3.27 vs 2.76), with no meaningful difference in return rates.
- **Which time periods had the most sales?** 2022-08 - This dataset only has `signup_date`, not an actual purchase/order date, so any time-based pattern here reflects when customers joined, not when they bought.
- **Is there anything in the data that could lead to a wrong conclusion?** Yes — worth keeping in mind for later analysis:
  - The 6 rows with `returned_items > purchase_count` and the 1 flagged `total_spending` mismatch were left uncorrected, so totals that include them carry a small amount of uncertainty.
  - `signup_date` isn’t a purchase date, so time-based conclusions are weaker than they might look.
  - No product-level data exists, so “what sells” can’t be answered from this file.
  - Only 60 customers total — patterns for smaller subgroups (a specific city, a specific tier) are directional, not statistically solid.

**A couple of extra things checked beyond the required questions:**

- **Spending by membership tier** — Silver and Gold customers actually spend more on average than VIP customers, which is a bit counterintuitive and worth a second look (see the notebook).