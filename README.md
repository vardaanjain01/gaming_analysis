# Indian Gaming & Esports Ecosystem Analysis (2021–2026)

## 1. Project Overview
India's gaming market has grown quickly across mobile, PC, console and cloud platforms. This project analyses **1,000,000 synthetic player records** to find where the most valuable players are, how the market has changed since 2021, and which players are most likely to churn.

**Role / audience:** I'm acting as a data analyst advising the marketing and product teams of a game publisher that wants to decide where to invest in India.

**Goal:** Identify the highest-value player segments (by state, platform, genre and age), understand what separates esports players from other players, measure churn risk by segment, and turn the findings into practical recommendations.

**Tools:** Python (pandas, NumPy, matplotlib, seaborn, SciPy, scikit-learn) and Excel (aggregated tables, native charts, dashboard).

---

## 2. Business Questions
Every chart, table and test in this project exists to answer one of these questions.

| # | Question | Why it matters |
|---|----------|----------------|
| Q1 | **Market structure:** Which states, platforms and genres account for the most players, total spend and play time? | Shows where the market is concentrated and where to focus. |
| Q2 | **Growth over time:** How did the player base and platform mix change across the 2021–2026 signup cohorts? | Reveals which platforms are growing and which are fading. |
| Q3 | **Monetisation:** Which platform × genre combinations earn the most per player and per hour played? | Helps decide where pricing and in-app purchase efforts pay off. |
| Q4 | **Demographics:** How do spend and play time differ by age group and gender? | Supports targeting and campaign design. |
| Q5 | **Esports:** How do esports participants differ from other players in spend, hours, age and genre? Are they worth targeting? | Informs esports sponsorship and tournament investment. |
| Q6 | **Churn:** Which segments have the highest churn risk, and do hours or spend relate to it? | Directs retention budgets to the right players. |
| Q7 | **Personas:** Can players be grouped into a few distinct types based on age, hours and spend? | Gives the business named, actionable audience groups. |

---

## 3. Dataset
**Source:** "Indian Gaming Industry Big Data" (synthetic dataset covering 2021–2026, described as 50 million player records).
**Used here:** a 1,000,000-row file (`indian_gaming_industry_bigdata_50m.csv`, player IDs `IND_GAMER_049000001` to `IND_GAMER_050000000`), which is the last 1M-record chunk of the full dataset.
**Note:** The file has no header row, so column names are assigned during loading.

| Column | Type | Description |
|--------|------|-------------|
| `player_id` | text | Unique player identifier |
| `age` | integer | Player age |
| `gender` | category | Male / Female / Other |
| `state` | category | One of 11 Indian states / regions |
| `platform` | category | Mobile (Android/iOS), PC (Steam/Epic), Console (PS5/Xbox), Cloud Gaming |
| `genre` | category | Battle Royale, Esports & Tactical Shooters, Cricket & Sports Simulation, Action-RPG & Souls-like, Casual/Puzzle/Strategy |
| `daily_hours` | float | Daily engagement hours |
| `spend_inr` | float | In-app purchase spending in INR |
| `signup_year` | integer | Signup cohort year (2021–2026) |
| `esports_participant` | boolean | Whether the player takes part in esports |
| `churn_risk` | boolean | Whether the player is flagged as at risk of churning |

---

## 4. Data Quality Notes (validation results on the raw file)

| Check | Result |
|-------|--------|
| Rows × columns | 1,000,000 × 11 |
| Missing values | 0 |
| Duplicate player IDs | 0 |
| Age range | 12 – 47 |
| Players under 18 | 166,654 (16.7%) |
| `daily_hours` range / mean | 0.0 – 29.3 / 2.20 |
| Rows with `daily_hours` > 24 (impossible) | 16 |
| Rows with `daily_hours` > 12 (extreme) | 4,217 |
| Rows with `daily_hours` = 0 | 22,406 |
| `spend_inr` range / mean | ₹0 – ₹7,500 / ₹3,750.29 |
| Players with zero spend | 1 |
| Signup years | 2021 – 2026 |
| Esports participants | 11.9% |
| Churn-risk flagged | 22.0% |

Observations:
- Spend is capped at ₹7,500 and almost everyone spends, so "spender vs non-spender" is not a useful split. Spend tiers are used instead.
- Under-18 players are about 1 in 6 and evenly spread across signup years, so they are kept and analysed as their own age group.
- The data is synthetic, so some relationships may be weak or absent. For example, average hours and spend are almost identical for churn-risk and non-churn-risk players. Findings are reported honestly, including "no meaningful difference found".

*(Final cleaning decisions are recorded in Section 6 once Step 4 is complete.)*

---

## 5. Methodology
1. **Setup and loading:** assign column names, set categorical data types, check memory use.
2. **Cleaning and validation:** handle impossible and extreme values, check consistency, save a clean file without touching the raw data.
3. **Feature engineering:** age groups, spend tiers, spend per hour, region groups, signup cohort labels.
4. **Exploratory analysis:** distributions, group comparisons, cohort trends, correlations.
5. **Statistical testing:** ANOVA / Kruskal-Wallis, t-tests / Mann-Whitney, chi-square, with **effect sizes and confidence intervals** reported alongside p-values (with 1M rows almost everything is "significant").
6. **Modelling:** logistic regression or decision tree for churn risk; K-means clustering for player personas.
7. **Reporting:** Excel workbook with KPI summary, aggregated tables, native charts and a data dictionary; written insights and recommendations.

---

## 6. Key Findings
*To be completed after the analysis. Each finding will state a number and link back to a question (Q1–Q7).*

- Q1: _TBD_
- Q2: _TBD_
- Q3: _TBD_
- Q4: _TBD_
- Q5: _TBD_
- Q6: _TBD_
- Q7: _TBD_

## 7. Recommendations
*To be completed after the analysis.*

## 8. Limitations
- The data is synthetic, so conclusions describe this dataset, not the real Indian gaming market.
- Only a 1M-row sample of the full 50M-row dataset is analysed.
- Column definitions come from the dataset description and were not independently verified.
- Observed relationships are associations, not proof of cause and effect.

---

## 9. Project Structure
```
gaming-analysis/
├── README.md
├── data/
│   ├── raw/          # original CSV, never edited
│   └── clean/        # cleaned dataset
├── notebooks/
│   ├── 01_cleaning.ipynb
│   ├── 02_eda.ipynb
│   └── 03_stats_and_models.ipynb
└── outputs/
    ├── charts/
    └── gaming_analysis_report.xlsx
```

## 10. How to Run
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn openpyxl xlsxwriter jupyter
# place the CSV in data/raw/, then run the notebooks in order (01 → 03)
```

## 11. Author
_Your name · your email / LinkedIn · GitHub link_
