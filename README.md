# Predicting 30-Day Hospital Readmissions in Diabetic Patients

> An end-to-end data analysis: from a raw clinical database to a model, a dashboard, and a recommendation a hospital could act on.

![status](https://img.shields.io/badge/status-in%20progress-blue) ![python](https://img.shields.io/badge/python-3.11-blue) ![license](https://img.shields.io/badge/license-MIT-green)

---

## The question

Hospitals are financially penalized when patients are readmitted within 30 days of discharge — under the U.S. Centers for Medicare & Medicaid Services (CMS) Hospital Readmissions Reduction Program, readmissions are treated as a signal of care quality and cost. Diabetic inpatients are a high-risk group.

**This project helps answer this question:**

> *Which diabetic patients are most likely to be readmitted within 30 days, what factors drive that risk, and where should a care-management team focus to reduce it?*


---

## The data

**Dataset:** Diabetes 130-US Hospitals for Years 1999–2008
**Source:** UCI Machine Learning Repository (dataset 296) — https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008
**License:** CC BY 4.0 (free to use with attribution)

| Property | Detail |
|---|---|
| Records | ~101,766 inpatient encounters |
| Features | ~50 (demographics, diagnoses, medications, prior visits, labs) |
| Target | `readmitted` — original levels `<30`, `>30`, `NO` |
| Span | 10 years of care across 130 hospitals |
| Origin | Health Facts national clinical data warehouse |

**Why this dataset (and not a clean Kaggle CSV):** it is realistically messy, which is the point.
- Missing values are coded as `?`, not blanks — they won't be caught automatically.
- Some columns are nearly unusable (`weight` and `max_glu_serum` are ~95% missing) and the *decision to drop them* is part of the analysis.
- Diagnoses are raw ICD-9 codes (hundreds of distinct values) that must be grouped into clinical categories (circulatory, respiratory, diabetes, etc.) before they're useful.
- It contains sensitive attributes (race, age, gender), which forces an honest fairness check — a strong differentiator in a healthcare portfolio.

> **Modeling note:** the three-class target is usually reframed as a binary problem — `<30 days` (readmitted) vs everything else — because the 30-day window is what CMS penalizes. State this choice explicitly; reviewers look for it.

---

## What this project demonstrates

| Stage | Skill shown | Tools |
|---|---|---|
| Frame the question | Domain framing, business sense | — |
| Load into a database & query | SQL, joins | SQLite / PostgreSQL |
| Clean & wrangle | Data cleaning, `?`-handling, ICD-9 grouping | Python, pandas |
| Explore & test | EDA, hypothesis testing | pandas, scipy, seaborn |
| Model | ML literacy, evaluation, interpretation | scikit-learn |
| Fairness check | Responsible/ethical analysis | fairlearn (optional) |
| Visualize | Dashboarding, data storytelling | Tableau / Power BI |
| Communicate | Written recommendation | this README + report |

---

## Key findings

*(Fill these in after your analysis — keep them specific and tied to a decision. Examples of the shape they should take:)*

- The 30-day readmission rate in the cohort was **[X]%**.
- The strongest predictors of readmission were **[e.g. number of prior inpatient visits, number of diagnoses, discharge disposition]**.
- Patients with **[trait]** were **[N]×** more likely to be readmitted — a group a care-management program could target first.
- A logistic-regression baseline reached **[ROC-AUC]**; a gradient-boosted model improved it to **[ROC-AUC]**, but the simpler model is easier to explain to clinicians.
- A fairness check across race/age groups showed **[finding]**, which matters before any model informs care decisions.

**Recommendation:** *[1–2 sentences. e.g. "Prioritize follow-up outreach for patients with 2+ prior inpatient visits and 8+ active diagnoses; this group is X% of discharges but Y% of readmissions, making it the highest-leverage place to intervene."]*

---

## Dashboard

*(Link your published Tableau Public / Power BI dashboard here, with a screenshot.)*

The dashboard lets a non-technical reader answer three questions in under ten seconds: *how big is the readmission problem, who is most at risk, and where should we act first?*

![dashboard preview](reports/dashboard_preview.png)

---

## How to reproduce

```bash
# 1. Clone and set up
git clone https://github.com/<you>/diabetes-readmission.git
cd diabetes-readmission
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# 2. Download the data
#    Get the zip from the UCI link above and place diabetic_data.csv in data/raw/

# 3. Run the pipeline
python src/01_load_to_db.py        # load raw CSV into SQLite
python src/02_clean.py             # handle '?', group ICD-9 codes, engineer features
python src/03_explore.py           # EDA + statistical tests -> figures
python src/04_model.py             # train, evaluate, interpret
```

---

## Repository structure

```
diabetes-readmission/
├── data/
│   ├── raw/              # original CSV (not committed — see .gitignore)
│   └── processed/        # cleaned output
├── notebooks/
│   └── exploration.ipynb # narrative EDA
├── sql/
│   └── cohort.sql        # the SQL that builds the analysis cohort
├── src/
│   ├── 01_load_to_db.py
│   ├── 02_clean.py
│   ├── 03_explore.py
│   └── 04_model.py
├── reports/
│   ├── findings.md        # the written recommendation
│   └── dashboard_preview.png
├── requirements.txt
└── README.md
```

---

## Tech stack

`Python` · `pandas` · `scikit-learn` · `SQL (SQLite)` · `matplotlib` / `seaborn` · `Tableau` (or `Power BI`)

---

## Limitations & honesty

- The data covers **1999–2008**, so absolute rates are historical; treat this as a methodology and reasoning demonstration, not a current clinical claim.
- This is observational data: the model finds **associations, not causes**. Any recommendation is about *where to look*, not proof that an intervention will work.
- Several features are heavily missing; dropped columns and imputation choices are documented in `src/02_clean.py`.
- A predictive model touching protected attributes should never drive care decisions without clinical oversight and a fairness review — included here as a discussion point.

---

## About

Built by Jeremy Rummel as a demonstration of an end-to-end analytics workflow — sourcing, cleaning, analyzing, modeling, and communicating real-world clinical data.
📧 jfrummel@gmail.com · 🔗 https://www.linkedin.com/in/jeremy-rummel-73ab3356/ · 📊 [Dashboard]

*Data: Strack et al. (2014), "Impact of HbA1c Measurement on Hospital Readmission Rates," BioMed Research International. Dataset via UCI ML Repository, CC BY 4.0.*
