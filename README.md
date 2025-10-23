# Data-cleaning-beginner-
# 1. Problem Statement
The goal of this project is to clean and perform exploratory data analysis (EDA) on a dataset

# Data Understanding

This section summarizes what we know about the dataset and highlights the key items to inspect before and during cleaning.

- **Dataset overview**
  - Source file: `dirty_cafe_sales.csv` (sales transactions from a cafe).
  - Typical columns: `Item`, `Quantity`, `Price Per Unit`, `Total Spent`, `Payment Method`, `Location`, `Transaction Date`.
  - Working row count and column list are available from the earlier `df.shape` and `df.columns` outputs.

- **Column types & what they mean**
  - `Transaction Date` — date/time of the sale. Convert to datetime and inspect for parsing errors or impossible dates.
  - `Item` — product name (categorical / free text). Expect spelling variants, case differences, and mislabeled values (e.g., `ERROR`, `NaN`).
  - `Quantity` — number of units sold (numeric). May contain non-numeric entries or missing values.
  - `Price Per Unit` — unit price (numeric). Check for currency symbols, thousands separators, and outliers.
  - `Total Spent` — total transaction amount (numeric). Can be used to validate `Quantity * Price Per Unit`.
  - `Payment Method` — categorical (e.g., Cash, Card). Useful for segmentation.
  - `Location` — store or POS location (categorical). Useful for comparing regions.

- **Missing values & special tokens**
  - We observed `NaN` and tokens like `ERROR` in several columns. The cleaning function replaced these with `Unknown` for categorical columns and coerced numerics with `errors='coerce'`.
  - After coercion, re-check `df.isna().sum()` to see remaining gaps (especially in `Transaction Date`, `Quantity`, `Price Per Unit`).

- **Distributions & outliers to check**
  - Price, Quantity, and Total Spent: inspect summary statistics (`.describe()`), histograms, and boxplots for skew and extreme values.
  - Look for transactions with Quantity <= 0 or Price Per Unit <= 0 — these are likely errors.
  - Compare `Revenue = Quantity * Price Per Unit` with `Total Spent` to find mismatches or record-level inconsistencies.

- **Relationships to explore**
  - Correlation between `Price Per Unit`, `Quantity`, and `Total Spent` to spot weird patterns.
  - Time series (daily/weekly) of revenue to identify seasonality, trends, or gaps in data.
  - Top items by count and by revenue — helps prioritize cleaning for the most impactful products.
  - Payment method and location breakdowns for operational insights.

- **Immediate findings to act on (next steps)**
  1. Run the numeric coercion and create a `Revenue` column (we did this in preprocessing). Recompute `df.isna().sum()` and inspect rows with missing critical fields.
  2. Investigate and fix obvious outliers or invalid rows (e.g., negative prices, non-sensical dates).
  3. Normalize `Item` text values (case, spacing, common typos) for accurate grouping.
  4. Produce the visualizations (time series, top items, distributions, heatmap) to verify data shape and highlight remaining issues.

- **Quality checks / validation rules**
  - `Quantity` and `Price Per Unit` should be non-negative numbers.
  - `Total Spent` should be close to `Quantity * Price Per Unit` (allow small rounding differences).
  - `Transaction Date` should fall within the expected operational period for the cafe.

This summary should guide the cleaning steps and the exploratory visualizations that follow. 

# Data cleaning
For datacleaning clean function was used since the features had similar errorsand for faster and clean code
Here is the code:

# Creating a function that cleans the column. (Replacing the missing values with 'Unknown')
    def clean(column):
        #Getting the unique values and their count
        print(f"VALUE COUNTS BEFORE CHANGES\n{column.value_counts(dropna= False)}")

        # Replacing the missing values in the 'Item' column with 'Unknown'
        column = column.replace(to_replace= np.nan, value= 'Unknown')

        # Replacing 'ERROR' with 'Unknown'
        column = column.replace(to_replace= 'ERROR', value= 'Unknown')

        # Changing the format of all values to Title case
        column = column.str.title()

        # Getting the unique values and their count after changes
        print('\n')
        print(f"VALUE COUNTS AFTER CHANGES\n{column.value_counts(dropna= False)}")

        return column 




