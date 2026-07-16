# NBA Players — Exploratory Data Analysis (EDA)

A beginner-to-intermediate data analysis project exploring a dataset of NBA
players, built using **only NumPy, Pandas, and Matplotlib** (no Seaborn,
Plotly, or scikit-learn).

## What this project does

This project takes a raw CSV of 458 NBA players (name, team, position, age,
height, weight, college, salary) and answers real questions about the league:

- How are age, height, weight, and salary distributed among players?
- Which teams have the most players / the highest total payroll?
- Which positions earn the most on average?
- Is there a relationship between a player's age and their salary?
- Is there a relationship between height and weight?
- Which colleges send the most players to the NBA?

## Why this dataset needed cleaning

Real-world data is never perfectly clean, and explaining *why* is part of
understanding the project:

| Issue | Why it happens | How it was handled |
|---|---|---|
| 1 fully blank row at the end of the file | Common artifact from CSV exports | Dropped with `dropna(how="all")` |
| Missing `Salary` values | Some players are on two-way / minimum / summer contracts not listed in this dataset's source | Filled with the **median** salary (median is used instead of mean because salary is right-skewed by a few very high earners — the mean would be distorted) |
| Missing `College` values | Some players went straight from high school to the NBA (common before the "one-and-done" rule), or are international players with no US college | Filled with the label `"None"` instead of dropped, so those players aren't lost from the analysis |
| `Height` stored as text like `"6-2"` | Feet-inches notation isn't numeric, so no math (averages, comparisons) can be done on it directly | Converted to a new column `Height_in` = total height in inches (`6-2` → 74) |

## Project structure

```
nba_eda_project/
├── NBA_EDA.ipynb        # Main analysis notebook (all code + charts + explanations)
├── data/
│   └── nba.csv           # Raw dataset
├── images/                # Exported chart images (also embedded in the notebook)
│   ├── 01_univariate_distributions.png
│   ├── 02_players_per_team.png
│   ├── 03_players_per_position.png
│   ├── 04_top_colleges.png
│   ├── 05_age_vs_salary.png
│   ├── 06_height_vs_weight.png
│   ├── 07_avg_salary_by_position.png
│   ├── 08_top_team_payroll.png
│   └── 09_salary_by_age_group.png
├── requirements.txt
└── README.md
```

## How the analysis is organized (the notebook's 6 sections)

1. **Data Loading & Inspection** — load the CSV, check shape, dtypes, and
   count missing values per column with `.isnull().sum()`.
2. **Data Cleaning** — fix the height format, fill missing salaries/colleges
   (see table above).
3. **Univariate Analysis** — histograms of Age, Height, Weight, Salary, plus
   manual mean/median/std/variance calculations using `np.mean`, `np.median`,
   `np.std`, `np.var` (done manually with NumPy rather than only relying on
   `.describe()`, to practice the underlying statistics).
4. **Categorical Analysis** — bar charts for players per team, per position,
   and top colleges, using `.value_counts()`.
5. **Bivariate / Correlation Analysis** — scatter plots and `np.corrcoef` to
   quantify relationships (age vs salary, height vs weight), plus grouped
   averages (`.groupby()`) for salary by position, payroll by team, and
   salary by age bucket (using `pd.cut`).
6. **Key Insights Summary** — a written recap of what the numbers mean.

## Key findings

- **Salary is right-skewed**: a small number of star players earn far above
  the median, which is exactly why the median (not the mean) was used to
  fill missing salary values.
- **Age and Salary have a weak-to-moderate positive correlation (r ≈ 0.21)** —
  older players tend to earn somewhat more, likely reflecting experience and
  long-term contracts, but age alone doesn't explain most of the variation in pay.
- **Height and Weight are strongly correlated (r ≈ 0.83)** — unsurprising,
  since taller athletes generally carry more mass.
- **Centers (C) earn the most on average (~$5.97M)**, followed by Point
  Guards (~$4.98M), Small Forwards (~$4.83M), Power Forwards (~$4.51M), and
  Shooting Guards (~$3.98M) in this dataset.
- **The Cleveland Cavaliers, LA Clippers, and Oklahoma City Thunder** had the
  three highest total team payrolls in this snapshot of the data.

*(Numbers are from this specific dataset snapshot — re-running the notebook
will reproduce them exactly.)*

## How to explain this project if someone asks

A simple way to describe it in conversation:

> "I took a dataset of NBA players and cleaned it up — handling missing
> salaries and college info, and converting height from feet-inches text
> into a usable number. Then I explored it with NumPy and Pandas: looked at
> how age, height, weight, and salary are distributed, compared teams and
> positions, and used correlation to check whether age predicts salary and
> whether height predicts weight. I visualized everything with Matplotlib.
> The main takeaway was that salary is very skewed by a few star players,
> and that centers earn the most on average by position."

## How to run it

```bash
pip install -r requirements.txt
jupyter notebook NBA_EDA.ipynb
```

## Tools used

- **Pandas** — loading, cleaning, grouping, aggregating tabular data
- **NumPy** — numeric array operations, manual statistics, correlation
- **Matplotlib** — all charts (histograms, bar charts, scatter plots)
