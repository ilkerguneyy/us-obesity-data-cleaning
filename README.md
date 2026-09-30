# U.S. Obesity Data — Cleaning & Visualization in Python

Cleaning a deliberately corrupted dataset in pandas and exploring obesity-related patterns with seaborn and plotly.

*Team project for STAT 112 (Introduction to Data Processing and Visualization), Department of Statistics,
METU — Fall 2024.*

## Data

A simulated dataset of 700 observations and 14 variables on obesity in the United States (2000–2023): state,
age group, income level, gender, diet type, activity level, obesity level, BMI, weekly physical activity hours,
daily caloric intake, medical visits, screen time and stress level. See `dataset_description.txt` for the intended
ranges and categories.

The raw file (`dirty_obesity_levels_dataset.csv`) was intentionally corrupted: inconsistent column names, numeric
columns stored as text, truncated values (`20...`, `Ala...`), character substitutions (`Al@b@m@`, `Conn3cticut`),
inconsistent casing and whitespace, negative and out-of-range values, and 2–5% missing values per column.

## Cleaning steps

All cleaning is in the first half of `clean_and_viz.ipynb`.

- **Structure:** renamed columns to match the data description; checked duplicates and missing values
  (dropping rows was not an option — it would have removed more than 10% of the data).
- **Numeric columns:** stripped truncation markers, converted to numeric types, imputed missing values with the
  median; negative BMI and calorie values were recovered as absolute values, and calorie entries that were
  one-tenth of the valid range were rescaled.
- **Medical visits:** a count variable (Poisson, mean 2 by design), so missing values were imputed by sampling
  from the observed distribution rather than with a single constant.
- **Categorical columns:** mapped character substitutions back to letters (`@`→`a`, `3`→`e`, …), then used fuzzy
  matching against lists of valid values (`difflib`) to repair misspelled and truncated categories; `us_states.csv` is the reference list for state names.
- **Consistency:** `ObesityLevel` and `ActivityLevel` turned out to be inconsistent with the numeric columns, so
  they were re-derived from BMI and physical activity hours using standard cut-offs.

The result is `cleaned_data.csv`.

## Research questions

Each team member explored one question on the cleaned data (second half of the notebook):

1. How do economic status and gender relate to obesity rates?
2. How does physical activity relate to calorie intake across obesity levels?
3. How does screen time relate to stress, and does this vary across states?
4. **Among adults, how do gender and physical activity relate to BMI and obesity across income levels?** *(my question)*
5. How has BMI changed over the years?
6. Does the number of medical visits change across obesity levels, BMI and caloric intake?
7. How does physical activity relate to screen time across genders?

## My part

Question 4, restricted to the adult age group:

- obesity-level counts by income level (count plot),
- BMI against weekly physical activity hours, split by gender (scatter plot),
- BMI distribution by income level and gender (box plot).

## How to run

```bash
python -m venv .venv
.venv\Scripts\python -m pip install -r requirements.txt   # Windows; use .venv/bin/python on macOS/Linux
.venv\Scripts\jupyter notebook clean_and_viz.ipynb
```

Use pandas 2.x: pandas 3 changes how missing values are cast to strings and breaks the year-cleaning step.

## Notes

- `us_states.csv` was not preserved from the original project and was recreated (50 states with postal
  abbreviations) so the notebook runs end to end.
- The dataset is simulated, so the patterns found here describe the generated data, not real U.S. obesity trends.

## Team

Ahmet Doğan, Ali Altuntaş, Salih Boran Çakır, Abdulkadir Yörük, İlker Güney, Talha Mert Karakoç, Mehmet Karaman.
Original team repository: [ForxDeven/obesity_research](https://github.com/ForxDeven/obesity_research).
