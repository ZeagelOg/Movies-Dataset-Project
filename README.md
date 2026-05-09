# Movie Dataset — Data Science in Python

> Exploratory Data Analysis and Cleaning using Pandas & NumPy | IMDb Movie Database

---

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset Summary](#dataset-summary)
- [Files](#files)
- [Libraries Used](#libraries-used)
- [Task 1 — Reading and Inspection](#task-1--reading-and-inspection)
- [Task 2 — Cleaning the Data](#task-2--cleaning-the-data)
- [Task 3 — Data Analysis](#task-3--data-analysis)
- [Key Findings](#key-findings)

---

## Project Overview

This project performs end-to-end data analysis on an IMDb movie dataset using Python. It covers data importing, inspection, cleaning, and analysis — extracting meaningful insights about movie profitability, director quality, genre popularity, and actor performance.

---

## Dataset Summary

| Attribute | Detail |
|-----------|--------|
| File | `Movie_Assignment_Data.csv` |
| Total Rows | 5,042 movies |
| Total Columns | 28 |
| Key Columns | `gross`, `budget`, `imdb_score`, `director_name`, `genres`, `actor_1_name` |
| Source | IMDb Movie Database |

---

## Files

| File | Description |
|------|-------------|
| `Movie_Assignment_Data.csv` | Raw movie dataset |
| `DS_in_Python_Movie_Assignment_Solution.ipynb` | Jupyter Notebook with full solution |
| `README.md` | This file |

---

## Libraries Used

| Library | Type | Purpose |
|---------|------|---------|
| `pandas` | Third-party | Data manipulation and analysis |
| `numpy` | Third-party | Numerical operations and NaN handling |
| `warnings` | Python built-in | Suppressing unnecessary warnings |

---

## Task 1 — Reading and Inspection

### Subtask 1.1 — Import and Read
- Imported the CSV file using `pd.read_csv()` and stored it in a dataframe called `movies`

### Subtask 1.2 — Inspect the Dataframe
- Checked shape using `movies.shape` — **5,042 rows x 28 columns**
- Inspected column types using `movies.info()`
- Generated descriptive statistics using `movies.describe()`

---

## Task 2 — Cleaning the Data

### Subtask 2.1 — Inspect Null Values
- Calculated column-wise and row-wise null counts using `isnull().sum()`
- Computed null percentage per column (rounded to 2 decimal places)

### Subtask 2.2 — Drop Unnecessary Columns
The following 15 columns were dropped as irrelevant to the analysis:

| Columns Dropped |
|----------------|
| `color`, `director_facebook_likes`, `actor_1_facebook_likes`, `actor_2_facebook_likes` |
| `actor_3_facebook_likes`, `actor_2_name`, `actor_3_name`, `cast_total_facebook_likes` |
| `duration`, `facenumber_in_poster`, `content_rating`, `country` |
| `movie_imdb_link`, `aspect_ratio`, `plot_keywords` |

Remaining columns after drop: **13**

### Subtask 2.3 — Drop Rows with High NaN Percentage
- Columns `gross` and `budget` had more than 5% null values
- All rows with NaN in either column were removed using `np.isnan()` with negation `~`

### Subtask 2.4 — Drop Rows with Too Many Nulls
- Retained only rows where the total number of null values per row was **5 or fewer**

### Subtask 2.5 — Fill Remaining NaN Values
- The `language` column had a small number of missing values
- All NaN entries in `language` were filled with `'English'` (the dominant language in the dataset)

### Subtask 2.6 — Rows Retained After Cleaning

| Metric | Value |
|--------|-------|
| Rows retained | ~3,807 |
| Percentage of original data retained | ~75.5% |
| Remaining NaN columns | `num_critic_for_reviews`, `actor_1_name` (small percentage, left as-is) |

---

## Task 3 — Data Analysis

### Subtask 3.1 — Unit Conversion
- `gross` and `budget` columns converted from `$` to `million $` by dividing by 1,000,000

### Subtask 3.2 and 3.3 — Top 10 Most Profitable Movies
- Created a new `profit` column: `profit = gross - budget`
- Sorted dataframe by `profit` in descending order
- Dropped duplicate rows using `drop_duplicates(keep='first')`
- Extracted the **top 10 most profitable movies**

### Subtask 3.4 — IMDb Top 250
- Filtered movies with `num_voted_users > 25,000`
- Sorted by `imdb_score` in descending order
- Extracted top 250 movies and assigned a `Rank` column (1 to 250)
- Extracted non-English films into a separate dataframe: `Top_Foreign_Lang_Film`

### Subtask 3.5 — Best Directors
- Created a pivot table grouping by `director_name` with mean `imdb_score`
- Extracted the **top 10 directors** by average IMDb score

### Subtask 3.6 — Popular Genre Combinations
- Split the `genres` column (pipe-separated) into `genre_1` and `genre_2`
- Movies with only one genre had the same value assigned to both columns
- Grouped by `genre_1` and `genre_2`, calculated mean gross revenue
- Identified the **top 5 most profitable genre combinations**

### Subtask 3.7 — Critic-Favorite vs Audience-Favorite Actors
- Extracted movies where the lead actor (`actor_1_name`) was Meryl Streep, Leonardo DiCaprio, or Brad Pitt
- Combined all three into a single dataframe using `pd.concat()`
- Grouped by `actor_1_name` and compared mean values of:
  - `num_critic_for_reviews` — critic engagement
  - `num_user_for_reviews` — audience engagement

---

## Key Findings

### Top 10 Profitable Movies
- Calculated after unit conversion and duplicate removal
- Profit = Gross revenue minus Production budget (both in million $)

### Directors
- Top 10 directors ranked by average IMDb score using a pivot table

### Genre Combinations
- Top 5 genre combos identified by mean gross revenue across all movies in that combination

### Actor Comparison

| Actor | Critic Reviews (Mean) | User Reviews (Mean) |
|-------|----------------------|---------------------|
| Leonardo DiCaprio | Highest | Highest |
| Meryl Streep | Strong | Moderate |
| Brad Pitt | Moderate | Moderate |

> Leonardo DiCaprio ranked highest in both critic and user review counts among the three actors analyzed.
