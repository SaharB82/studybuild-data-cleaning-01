# Project 01 — Data Cleaning on an E-commerce Dataset

## 1. Project Overview

This project focuses on **Data Cleaning and Data Quality Assessment** of an e-commerce customer dataset.

The purpose of the project is to transform a raw dataset into a clean, consistent, reliable, and analysis-ready dataset.

According to the project requirements, the main tasks include:

* Understanding the raw dataset
* Identifying Data Quality issues
* Making appropriate decisions for each issue
* Cleaning and standardizing the data
* Documenting all changes and decisions
* Producing a final cleaned dataset

The final cleaned dataset is intended to be used for further analysis of customer behavior, purchasing patterns, and business performance.

---

# 2. Initial Dataset Understanding

## 2.1 Dataset Description

The dataset contains customer-level information from an e-commerce business.

Each row represents one customer, while the columns contain information about:

* Customer identification
* Customer demographics
* Location
* Membership status
* Purchase behavior
* Financial performance
* Recency of purchase
* Payment method
* Device usage
* Discount usage
* Returned items
* Customer satisfaction

After the initial inspection and removal of one duplicate record, the working dataset contains **60 customer records and 17 columns**.

The dataset includes both categorical and numerical variables, as well as a date field.

---

## 2.2 Dataset Structure

The dataset contains the following columns:

| Column               | Description                            | Data Type   |
| -------------------- | -------------------------------------- | ----------- |
| `customer_id`        | Unique identifier of the customer      | Integer     |
| `first_name`         | Customer's first name                  | String      |
| `gender`             | Customer gender                        | Categorical |
| `age`                | Customer age                           | Integer     |
| `city`               | Customer city                          | Categorical |
| `province`           | Customer province                      | Categorical |
| `signup_date`        | Customer registration date             | Datetime    |
| `membership_tier`    | Customer membership level              | Categorical |
| `purchase_count`     | Number of purchases                    | Integer     |
| `avg_order_value`    | Average value of each order            | Float       |
| `total_spending`     | Total amount spent by the customer     | Float       |
| `last_purchase_days` | Number of days since the last purchase | Integer     |
| `payment_method`     | Customer's payment method              | Categorical |
| `device`             | Device used by the customer            | Categorical |
| `discount_used`      | Whether the customer used a discount   | Categorical |
| `returned_items`     | Number of returned items               | Integer     |
| `satisfaction_score` | Customer satisfaction score            | Integer     |

---

# 3. Initial Data Quality Assessment

The raw dataset was systematically checked for the main Data Quality issues specified in the project instructions.

The following areas were investigated:

1. Duplicate records
2. Missing values
3. Incorrect data types
4. Inconsistent date formats
5. Invalid values
6. Statistical outliers
7. Inconsistent categorical values
8. Logical inconsistencies between columns
9. Consistency of calculated financial values

---

# 4. Duplicate Records

## Problem Identified

One duplicate record was identified during the initial inspection of the dataset.

## Action Taken

The duplicate record was removed.

## Reason

Duplicate customer records can lead to:

* Incorrect customer counts
* Inflated purchase statistics
* Biased analysis
* Incorrect aggregation results

Therefore, duplicate records were removed before continuing with the cleaning process.

After removing the duplicate, **60 unique customer records remained**.

---

# 5. Missing Values

The dataset was checked using a column-level missing-value assessment.

Two missing values were identified.

## 5.1 Missing Value in `age`

A missing value was identified for:

* `customer_id`: 1020
* `first_name`: Kimia
* `age`: Missing

### Decision

The actual age could not be reliably inferred from the available data.

Therefore, the missing value was replaced using the **median age of the dataset**.

The calculated median age was:

**45**

Therefore:

`age: Missing → 45`

### Reason

Median imputation was selected because:

* Age is a numerical variable.
* The actual value could not be determined from other fields.
* The median is less sensitive to extreme values than the mean.
* The dataset contained an extreme age value that could affect the mean.

---

## 5.2 Missing Value in `total_spending`

A missing value was identified for:

* `customer_id`: 1040
* `first_name`: Amir
* `total_spending`: Missing

The dataset showed a consistent relationship between:

`total_spending = purchase_count × avg_order_value`

For customer 1040:

* `purchase_count = 34`
* `avg_order_value = 37.66`

Therefore:

`34 × 37.66 = 1280.44`

### Decision

The missing `total_spending` value was replaced with:

**1280.44**

### Reason

Unlike the missing age, the missing financial value could be calculated directly and accurately from two existing variables.

Therefore, calculated imputation was preferred over statistical imputation.

After these corrections:

**No Missing Values remained in the dataset.**

---

# 6. Data Type and Date Standardization

## 6.1 `signup_date`

Initially, the `signup_date` column was stored as a string.

The values were inspected to identify potential date-format inconsistencies.

All observed dates followed the same format:

`YYYY-MM-DD`

For example:

`2025-02-19`

Therefore, no inconsistent date formats were found.

However, because the column represented dates, it was converted from string to a proper datetime data type.

### Action

`signup_date`:

`String → Datetime`

### Reason

Using a datetime data type allows:

* Date-based analysis
* Sorting by date
* Date calculations
* Time-based filtering
* More reliable downstream analysis

---

## 6.2 `age`

After resolving the missing value in `age`, the column was converted from:

`float64 → int64`

### Reason

Age is a discrete integer variable and should not contain decimal values.

The column had initially been represented as `float64` because it contained a missing value.

---

# 7. Categorical Data Validation

The categorical variables were inspected for:

* Unexpected values
* Different capitalization
* Spelling inconsistencies
* Extra spaces
* Duplicate representations of the same category

The following columns were checked:

* `gender`
* `city`
* `province`
* `membership_tier`
* `payment_method`
* `device`
* `discount_used`

## Results

### `gender`

Valid values:

* `F`
* `M`

No inconsistent representation was identified.

### `city`

The dataset contains the following cities:

* Karaj
* Tehran
* Shiraz
* Mashhad
* Isfahan
* Rasht
* Tabriz
* Ahvaz

No obvious naming inconsistency was identified.

### `province`

The dataset contains:

* Alborz
* Tehran
* Fars
* Khorasan
* Isfahan
* Gilan
* East Azerbaijan
* Khuzestan

No obvious spelling or formatting inconsistency was identified.

### `membership_tier`

Valid membership levels:

* Bronze
* Silver
* Gold
* VIP

No inconsistent capitalization or duplicate category representation was identified.

### `payment_method`

Valid values:

* Card
* Cash
* Online Wallet

No inconsistency was identified.

### `device`

Valid values:

* Android
* Web
* iPhone

No inconsistency was identified.

### `discount_used`

Valid values:

* Yes
* No

No inconsistency was identified.

---

# 8. Invalid Values and Logical Inconsistencies

In addition to statistical checks, logical relationships between variables were examined.

## 8.1 Invalid Age

One extreme and logically invalid age was identified:

* `customer_id`: 1010
* `first_name`: Arash
* `age`: 145

The value was also identified as a statistical outlier using the IQR method.

### Decision

The value was replaced with the median age:

`145 → 45`

### Reason

An age of 145 is not considered a valid customer age and there was no reliable information available to determine the actual age.

Using the median was consistent with the strategy used for the missing age value.

---

# 9. Outlier Detection

The **Interquartile Range (IQR)** method was used to identify statistical outliers in numerical variables.

The following numerical columns were examined:

* `age`
* `purchase_count`
* `avg_order_value`
* `total_spending`
* `last_purchase_days`
* `returned_items`
* `satisfaction_score`

The IQR method defines:

`IQR = Q3 - Q1`

and identifies observations outside:

`Lower Bound = Q1 - 1.5 × IQR`

`Upper Bound = Q3 + 1.5 × IQR`

---

## 9.1 Outlier in `age`

The IQR analysis produced:

* Q1 = 31.75
* Q3 = 58
* IQR = 26.25
* Upper Bound = 97.375

The value `145` was therefore identified as an outlier.

Because this value was also logically invalid, it was corrected as described in Section 8.1.

---

## 9.2 Outliers in `total_spending`

Five statistical outliers were identified in `total_spending`.

However, statistical outlier detection alone does not mean that a value is incorrect.

Each of the five records was therefore investigated individually.

Four of the five outliers were consistent with:

`total_spending = purchase_count × avg_order_value`

Therefore, these values were considered **valid business outliers** and were retained.

### Valid Outliers Retained

Customers:

* 1009
* 1012
* 1044
* 1057

These customers had relatively high total spending, but their values were mathematically consistent with their purchase counts and average order values.

---

## 9.3 Incorrect `total_spending`

One outlier was identified as an actual data inconsistency:

* `customer_id`: 1030
* `purchase_count`: 26
* `avg_order_value`: 156.89
* Original `total_spending`: 25000

The expected value was:

`26 × 156.89 = 4079.14`

Therefore:

`25000 → 4079.14`

### Reason

The original value was not consistent with the mathematical relationship between the financial variables.

Unlike the other high-spending customers, this value was therefore treated as an incorrect data value rather than a valid outlier.

---

# 10. Logical Consistency Between Purchase and Return Data

The relationship between:

* `purchase_count`
* `returned_items`

was examined.

A logical rule was applied:

`returned_items ≤ purchase_count`

Six records violated this rule.

The identified inconsistencies were:

| Customer ID | Purchase Count | Original Returned Items | Corrected Returned Items |
| ----------: | -------------: | ----------------------: | -----------------------: |
|        1008 |              3 |                       8 |                        3 |
|        1015 |              3 |                       8 |                        3 |
|        1024 |              0 |                       2 |                        0 |
|        1029 |              2 |                       4 |                        2 |
|        1037 |              1 |                       7 |                        1 |
|        1056 |              1 |                       4 |                        1 |

### Decision

For these records, `returned_items` was capped at `purchase_count`.

### Reason

The original values violated a basic logical constraint: a customer's number of returned items cannot exceed the number of purchases recorded for that customer.

For example:

`purchase_count = 0` and `returned_items = 2`

is logically impossible under the definition of these fields.

The correction therefore used the maximum logically valid value:

`returned_items = purchase_count`

---

# 11. Financial Consistency Check

The relationship:

`total_spending = purchase_count × avg_order_value`

was checked after the cleaning process.

A temporary calculated field was created:

`calculated_spending = purchase_count × avg_order_value`

The values were compared using a floating-point tolerant comparison to avoid false differences caused by decimal precision.

The check confirmed that the cleaned `total_spending` values were consistent with the underlying purchase data.

The temporary calculation column was removed after validation.

---

# 12. Summary of Data Quality Issues and Actions

| Issue                       | Column(s)                           |     Records | Action                                      |
| --------------------------- | ----------------------------------- | ----------: | ------------------------------------------- |
| Duplicate record            | Entire row                          |           1 | Removed                                     |
| Missing value               | `age`                               |           1 | Replaced with median = 45                   |
| Missing value               | `total_spending`                    |           1 | Calculated as 1280.44                       |
| Incorrect data type         | `signup_date`                       | All records | Converted String → Datetime                 |
| Data type standardization   | `age`                               | All records | Converted Float → Integer                   |
| Invalid age                 | `age`                               |           1 | 145 → 45                                    |
| Statistical outliers        | `total_spending`                    |           5 | Investigated individually                   |
| Valid financial outliers    | `total_spending`                    |           4 | Retained                                    |
| Incorrect financial value   | `total_spending`                    |           1 | 25000 → 4079.14                             |
| Logical inconsistency       | `returned_items` / `purchase_count` |           6 | `returned_items` capped at `purchase_count` |
| Categorical inconsistencies | Categorical columns                 |           0 | No correction required                      |

---

# 13. Final Data Quality Status

After applying the cleaning procedures, the dataset was rechecked for the major Data Quality issues.

The final dataset should satisfy the following conditions:

* No duplicate records
* No missing values
* Correct data types
* Standardized date representation
* No invalid customer ages
* No negative purchase-related values
* `returned_items ≤ purchase_count`
* `satisfaction_score` within the valid range of 1–5
* No negative financial values
* `total_spending` consistent with purchase count and average order value
* No unresolved categorical formatting issues

The cleaned dataset contains:

**60 customer records × 17 columns**

---

# 14. Tools and Libraries

The project was performed using Python and Jupyter Notebook.

Main tools and libraries:

* Python
* Jupyter Notebook
* Pandas
* NumPy

Pandas was primarily used for:

* Data inspection
* Missing-value detection
* Duplicate detection
* Data type conversion
* Data filtering
* Data validation
* Data transformation

NumPy was used for numerical comparison and floating-point tolerant validation.

---

# 15. Before vs. After

## Initial Dataset

The raw dataset contained several Data Quality issues, including:

* Duplicate record
* Missing age
* Missing total spending
* Incorrect data type for date
* Invalid age value
* Incorrect total spending value
* Logical inconsistencies between purchases and returned items
* Statistical outliers requiring investigation

## Cleaned Dataset

After cleaning:

* Duplicate record was removed
* Missing values were resolved
* Date field was standardized
* Numerical data types were corrected
* Invalid age was corrected
* Incorrect total spending was corrected
* Valid financial outliers were preserved
* Purchase/return inconsistencies were corrected
* Categorical values were validated
* Financial consistency was verified

The resulting dataset is ready for further analysis.

---

# 16. Cleaning Philosophy

An important principle followed throughout this project was:

> **Not every unusual value is an incorrect value.**

Statistical outlier detection was therefore used as an investigation tool rather than an automatic deletion rule.

For example, several customers had unusually high `total_spending`. These values were not removed because they were mathematically consistent with their purchase counts and average order values.

In contrast, the value `total_spending = 25000` for customer 1030 was corrected because it violated the known relationship between `purchase_count`, `avg_order_value`, and `total_spending`.

Similarly, the age of 145 was treated as invalid because it was both statistically extreme and logically implausible.

This approach ensures that the cleaning process preserves legitimate business information while correcting actual data quality problems.

---

# 17. Final Deliverables

The project deliverables include:

```text
project-01-data-cleaning/
│
├── data/
│   └── cleaned_dataset.xlsx
│
├── notebook/
│   └── data_cleaning.ipynb
│
└── README.md
```

The cleaned dataset is provided as:

`cleaned_dataset.xlsx`

The Jupyter Notebook contains the data inspection, cleaning, validation, and transformation steps.

---

# 18. Conclusion

The purpose of this project was not simply to remove missing values or duplicate records.

The main objective was to demonstrate a structured Data Quality workflow:

**Inspect → Identify → Decide → Clean → Validate → Document**

The raw e-commerce customer dataset was systematically inspected, Data Quality issues were identified, appropriate correction strategies were selected, and the final dataset was validated against logical and statistical rules.

The final result is a cleaner, more consistent, and more reliable dataset that can be used as a foundation for subsequent customer, sales, and business analysis.
