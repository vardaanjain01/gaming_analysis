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
Analysis of 999,984 cleaned player records (16 impossible rows removed). Total spend is about ₹375 crore, averaging ₹3,750 per player.

**The headline result:** the market differs in *size* from segment to segment, but not in *value per player*. Spend, play time and churn are almost identical in every group.

- **Q1 – Market structure:** Players are concentrated. Uttar Pradesh (16.0%), Maharashtra (14.0%) and Karnataka (10.0%) lead the states; Mobile has 77.9% of players (PC 14.0%, Console 6.0%, Cloud 2.0%); Battle Royale is the biggest genre (38.0%) and Casual/Puzzle/Strategy the smallest (10.0%). Because average spend is flat (₹3,737–₹3,770 across every state, platform and genre), total revenue simply follows player count.
- **Q2 – Growth over time:** New signups rose from 80,046 in 2021 to a peak of 250,695 in 2024, then fell to 220,076 in 2025 and 149,996 in 2026 (2026 may be a partial year). The platform mix is the same in every cohort, so no platform is gaining share.
- **Q3 – Monetisation:** No platform × genre pair stands out. Average spend ranges only from about ₹3,730 to ₹3,813 across all 20 combinations (a spread of ₹84).
- **Q4 – Demographics:** Age groups (₹3,735–₹3,756), genders (₹3,747–₹3,752) and daily hours (about 2.20 in every group) are practically identical. Under-18 players are 16.7% of the data and behave like everyone else.
- **Q5 – Esports:** 11.9% of players take part in esports. They match non-participants on spend (₹3,756 vs ₹3,750), daily hours (2.20 vs 2.20), age (29.5 vs 29.5) and genre mix (Cohen's d ≤ 0.003).
- **Q6 – Churn:** 22.0% of players are flagged as at risk, and every segment sits between 21.8% and 22.3%. A logistic regression (AUC 0.5006) and gradient boosting (AUC 0.4996) predict churn no better than random guessing, and accuracy (78.0%) equals the "always no churn" baseline.
- **Q7 – Personas:** K-means (k = 4) gives four descriptive segments: Older Light Players (33.1%, avg age 40.5, ₹3,812), Young Low Spenders (28.0%, age 23.0, ₹1,679), Young High Spenders (27.4%, age 22.4, ₹5,750) and Heavy Players (11.6%, 6.9 hours a day, ₹3,742). The silhouette score is about 0.29, so the groups overlap.
- **Statistical testing:** 25 tests were run (ANOVA, t-tests, chi-square). Two reach p < 0.05 but have negligible effect sizes (for example eta² = 0.000012), and none survives a Bonferroni correction (threshold 0.002).

## 7. Recommendations
These follow from the findings. If the data reflected the real market, they would apply as written; because it is synthetic, treat them as a demonstration of how the analysis would be used.

1. **Prioritise reach over targeting.** Value per player is the same everywhere, so revenue scales with player numbers. Focus acquisition budgets on the largest pools: Mobile first, and Uttar Pradesh, Maharashtra and Karnataka.
2. **Don't price or market by platform, genre, gender or state.** None of these variables separates high-value players from low-value ones.
3. **Use spend-based segments instead.** The Young High Spenders (₹5,750 average) and Young Low Spenders (₹1,679) are the only groups with real value differences. Design upsell offers for the second group and loyalty perks for the first.
4. **Plan 2026 growth carefully.** Signups have fallen since the 2024 peak; confirm whether 2026 is a partial year before treating it as a decline.
5. **Collect behavioural data before investing in retention.** Churn cannot be predicted from demographics, platform, genre, hours or spend. Add session frequency, days since last login and purchase history to build a model that works.
6. **Don't expect an esports premium.** Esports participants spend the same as other players, so justify esports investment on engagement or brand goals, not on spend.

## 8. Limitations
- The data is synthetic, so conclusions describe this dataset, not the real Indian gaming market.
- Only a 1M-row sample of the full 50M-row dataset is analysed.
- Column definitions come from the dataset description and were not independently verified.
- Observed relationships are associations, not proof of cause and effect.
- Most attributes appear to be generated independently of each other, so the absence of relationships (flat spend, flat churn) is expected and is itself a finding.
- The 2026 signup count may reflect a partial year or the way the dataset was built, not a real decline.
- K-means segments are descriptive (silhouette ≈ 0.29), not naturally distinct player types.
- About 25 tests were run, so occasional p-values below 0.05 are expected by chance; effect sizes and a Bonferroni correction were used to judge them.

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
│   ├── 03_stats.ipynb
│   └── 04_models.ipynb
└── outputs/
    ├── charts/
    ├── segment_profiles.csv
    └── gaming_analysis_report.xlsx   # 11 sheets: summary, aggregated tables, charts, stats tests, data dictionary
```

## 10. How to Run
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn openpyxl xlsxwriter jupyter
# place the CSV in data/raw/, then run the notebooks in order (01 → 04)
```

## 11. Author
Vardaan Jain · jainvardaan111@gmail.com · [LinkedIn](https://www.linkedin.com/in/vardaan-jain-bb2b812b0/) · [GitHub](https://github.com/Mugambo1)
