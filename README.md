# Netflix Movies and TV Shows — Data Cleaning Pipeline

This repository contains a Python data-cleaning workflow built in Jupyter Notebook (`netflix_dataset_cleaned.ipynb`) that prepares the raw Netflix catalog dataset (`netflix_titles.csv`) for downstream exploratory data analysis (EDA) and visualization.

---

## Project Overview

* **Input Dataset:** `netflix_titles.csv` (8,807 rows, 12 columns)
* **Output Dataset:** `netflix_titles_cleaned.csv` (8,790 rows, 16 columns)
* **Tools Used:** Python 3, Pandas, JupyterLab

---

## Data Cleaning & Transformation Steps

1. **Initial Inspection & Duplicate Check**
   * Audited column data types, non-null counts, and summary statistics using `.info()` and `.describe()`.
   * Verified dataset integrity with `.duplicated().sum()`, confirming **0 duplicate records**.

2. **Missing Value Handling**
   * **Imputed Text Fields:** Replaced missing values in `director` (2,634 nulls), `cast` (825 nulls), and `country` (831 nulls) with `'unknown'` to preserve catalog records without losing rows.
   * **Dropped Incomplete Records:** Removed 17 rows with missing values in critical fields (`date_added`, `rating`, and `duration`), bringing the dataset from 8,807 to **8,790 complete rows** with zero remaining nulls.

3. **Date Parsing & Feature Engineering**
   * Stripped leading whitespace from `date_added` and converted strings into `datetime64` objects using `pd.to_datetime()`.
   * Extracted `month_added` (month name) and `year_added` (integer year) to enable time-series analysis.

4. **Duration Normalization**
   * Split the mixed-format `duration` column (e.g., `"90 min"`, `"2 Seasons"`) into two distinct analytical columns:
     * `duration_num` (`int64`): Numeric length of the movie or show.
     * `duration_unit` (`str`): Unit of measurement (`min`, `Season`, `Seasons`).

5. **Data Export**
   * Exported the finalized, null-free DataFrame to `netflix_titles_cleaned.csv` with `index=False`.

---

## Final Schema (`netflix_titles_cleaned.csv`)

| Column | Data Type | Description |
| :--- | :--- | :--- |
| `show_id` | String | Unique identifier for each title |
| `type` | String | Category (`Movie` or `TV Show`) |
| `title` | String | Name of the movie or TV show |
| `director` | String | Director(s) (`'unknown'` if unavailable) |
| `cast` | String | Actors involved (`'unknown'` if unavailable) |
| `country` | String | Production country/countries (`'unknown'` if unavailable) |
| `date_added` | Datetime | Date the title was added to Netflix |
| `release_year` | Integer | Original release year |
| `rating` | String | TV or movie maturity rating |
| `duration` | String | Original duration string |
| `listed_in` | String | Assigned genres/categories |
| `description` | String | Short synopsis |
| `month_added` | String | Extracted month name from `date_added` |
| `year_added` | Integer | Extracted year from `date_added` |
| `duration_num` | Integer | Numeric duration value |
| `duration_unit` | String | Duration unit (`min`, `Season`, `Seasons`) |

---

## How to Run

1. Clone this repository and ensure `netflix_titles.csv` is in the root directory.
2. Install Pandas if not already installed:
   ```bash
   pip install pandas
