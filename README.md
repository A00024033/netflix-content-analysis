# Netflix Content Strategy Analysis

Python data science project analysing Netflix's content catalogue to uncover patterns in content type, audience ratings, and regional production — framed as a media analytics consultancy brief.

## Overview

This project investigates 8,807 Netflix titles (movies and TV shows) using the [Netflix Movies and TV Shows dataset](https://www.kaggle.com/datasets/shivamb/netflix-shows) from Kaggle.

**Central hypothesis:** Netflix's content catalogue contains meaningful patterns in content type, audience rating, and geographic origin that can inform acquisition and platform growth strategy.

## Problem Statements

| # | Problem | Approach |
|---|---------|----------|
| 1 | Can we classify a title as Movie or TV Show using metadata? | Naive Bayes Classification |
| 2 | Has TV-MA rated content grown year-on-year (2015–2021)? | Trend Analysis |
| 3 | Do distinct geographic clusters exist by content type and genre? | Regional Analysis / EDA |

## Dataset

- **Source:** [Kaggle — Netflix Movies and TV Shows](https://www.kaggle.com/datasets/shivamb/netflix-shows)
- **Size:** 8,807 rows × 12 attributes
- **Coverage:** Netflix catalogue metadata up to mid-2021

## Pre-processing

7 documented preprocessing steps were applied:

| Step | Issue | Action |
|------|-------|--------|
| 1 | Duplicate rows | Checked — none found |
| 2 | Misplaced duration values in rating column | Moved to duration; rating set to NaN |
| 3 | Missing: director (29.91%), cast (9.37%), country (9.44%) | Filled with `'Unknown'` |
| 4 | Missing rating values (0.05%) | Imputed with mode: TV-MA |
| 5 | `date_added` as unformatted string | Converted to datetime; `year_added` and `month_added` extracted |
| 6 | `duration` mixes incompatible units | Split into `duration_minutes` and `duration_seasons` |
| 7 | `listed_in` contains multiple genre tags | Primary genre extracted as `primary_genre` |

**Final dataset:** 8,797 rows × 17 attributes (12 original + 5 engineered)

## Key Findings (EDA)

- Movies account for **69.6%** of titles vs 30.4% TV Shows (class ratio 2.3:1)
- **TV-MA** is the dominant rating at 36.5%
- **US** leads production with 2,815 titles, followed by India (972) and UK (419)
- **Dramas** are the most common genre (1,600 titles)
- TV-MA additions grew from 29 titles in 2015 to 738 in 2019 — a clear upward trend

## Technique: Naive Bayes Classification

**Why Naive Bayes?**
- Feature set is predominantly categorical — Categorical Naive Bayes handles this natively
- Low inter-feature correlation supports the conditional independence assumption
- Incorporates class priors directly, making it well-suited to the 2.3:1 class imbalance
- Produces probability estimates — interpretable to both technical and non-technical stakeholders

**Features used:**

| Feature | Type | Justification |
|---------|------|--------------|
| `rating` | Categorical | TV-MA/TV-14 strongly associated with TV Shows; R/PG-13 with Movies |
| `primary_genre` | Categorical | Genre distributions differ markedly by content type |
| `release_year` | Numeric | Older content skews Movie; TV Show additions grew post-2016 |
| `year_added` | Numeric | Netflix acquisition strategy shifted towards TV Shows over time |
| `month_added` | Numeric | Seasonal patterns in content additions may vary by type |

## Tech Stack

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-lightgrey)
![Scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)

- **Python** — pandas, NumPy, scikit-learn, matplotlib, seaborn
- **Jupyter Notebook** — full analysis and preprocessing pipeline
- **Categorical Naive Bayes** — via `sklearn.naive_bayes.CategoricalNB`

## Repository Structure

```
netflix-content-analysis/
├── data/
│   ├── netflix_titles.csv          # Raw dataset
│   └── netflix_titles_cleaned.csv  # Cleaned dataset
├── netflix_analysis.ipynb          # Full Jupyter notebook
├── report.pdf                      # Written report (CMP-N205-0)
└── README.md
```

## References

- Bansal, S. (2021). *Netflix Movies and TV Shows* [Dataset]. Kaggle.
- Mitchell, T. M. (1997). *Machine Learning*. McGraw-Hill.
- Pedregosa, F. et al. (2011). Scikit-learn: Machine Learning in Python. *JMLR*, 12, 2825–2830.
- McKinney, W. (2010). Data Structures for Statistical Computing in Python. *Proceedings of SciPy*, 51–56.
