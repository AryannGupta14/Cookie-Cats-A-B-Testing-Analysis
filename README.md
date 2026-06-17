# 🐱 Cookie Cats — A/B Testing Analysis

> My first A/B Testing end-to-end Data Science project as an MSc Statistics fresher.  
> Built to learn the full A/B testing pipeline — from data cleaning to Bayesian inference to ML prediction.

---

## 📌 What is this project about?

Cookie Cats is one of the world's most downloaded mobile puzzle games. Players move through levels interrupted by **gates** — forced stops requiring a wait or in-app purchase.

The game ran an experiment: **what happens if we move the gate from Level 30 → Level 40?**

This project analyzes that experiment using real data from **90,189 players** to answer:
> *Does gate position affect long-term player retention?*

---

## 📂 Files in this repo

| File | Description |
|------|-------------|
| `A_B_Testing_Project.ipynb` | Full analysis notebook |
| `A_B_Testing_analysis_Cookie_Cats.pdf` | Project presentation (slides) |
| cookie_cats.csv |  Dataset |

---

## 🔍 What I did

### 1. Data Cleaning & EDA
- Detected and removed a bot player (49,854 rounds — 46× the 99.9th percentile)
- Confirmed right-skewed distribution → used non-parametric tests
- Verified group balance via CDF overlays and summary statistics

### 2. Statistical Hypothesis Testing
| Test | Purpose | Result |
|------|---------|--------|
| SRM Check (Chi-sq) | Validate randomization | ✅ Pass — split was clean |
| Shapiro-Wilk | Normality check | ❌ Not normal → use non-parametric |
| Mann-Whitney U | Compare game rounds | Not significant |
| 2-Proportion Z-Test (D1) | Day 1 retention | Not significant |
| 2-Proportion Z-Test (D7) | Day 7 retention | ✅ Significant (p = 0.0016) |
| Chi-sq Independence (D7) | Confirm D7 result | ✅ Confirmed |

### 3. Effect Size & Bootstrap CI
- Cohen's h = 0.021 (small — big sample inflates significance)
- 95% Bootstrap CI: [0.0032, 0.0133] — entirely above zero
- 10,000 simulations → effect is real, not random noise

### 4. Bayesian A/B Testing
- Used Beta-Binomial model with uninformative prior Beta(1,1)
- **P(Gate 30 > Gate 40) = 99.9%**
- Business translation: *"We are 99.9% confident Gate 30 produces better 7-day retention"*

### 5. Segmentation Analysis
Gate effect is **not uniform** across player types:

| Segment | Rounds | Gate Effect Significant? |
|---------|--------|--------------------------|
| Zero | 0 | ❌ No |
| Casual | 1–10 | ❌ No (gate not yet encountered) |
| Moderate | 11–50 | ✅ Yes (p = 0.0007) |
| Engaged | 51–200 | ✅ Yes (p = 0.0001) |
| Power | 200+ | ❌ No (they adapt regardless) |

### 6. Survival Analysis (Kaplan-Meier)
- Adapted from medical research to track player "survival" past each gate
- Log-rank test: p = 0.023 → curves are significantly different
- Gate 30 players retain better across the entire progression journey

### 7. Predictive Modelling
Trained 3 models to predict 7-day retention:

| Model | ROC-AUC |
|-------|---------|
| Logistic Regression | 0.8888 |
| Random Forest | 0.8893 |
| XGBoost | 0.8895 |

Key finding: **log_gamerounds is the dominant predictor** — gate version ranks last.  
Translation: engagement drives retention far more than gate placement does.

---

## 📈 Key Results

- **Gate 30 wins on Day 7 retention** (19.02% vs 18.20%, p = 0.0016)
- Day 1 difference is not significant — the advantage builds over time
- Effect is real but small (Cohen's h = 0.021)
- The real Gate 30 benefit lives in **moderate and engaged players**, not casual or power users

---

## 💡 What I learned (honest version)

This was my first A/B testing project. Here's what genuinely clicked for me:

**SRM Check** — If randomization fails, every result downstream is invalid. This is now the first thing I check before anything else.

**Statistical vs Practical significance** — With 90K users, even tiny differences look significant. Cohen's h taught me to always report effect size alongside p-values.

**Bayesian thinking** — "p = 0.0016" confuses people. "99.9% probability Gate 30 is better" closes rooms. I now understand why Bayesian probability speaks better business language.

**Kaplan-Meier** — A technique from medical survival research. The connection between "patient surviving treatment" and "player surviving past a gate" is what made Data Science feel genuinely interesting to me.

**Segmentation matters** — The overall result was significant, but the *real* story was in the segments. Moderate and engaged players drove the entire effect. Aggregate results can mislead.

---

## 🛠️ Tech Stack

```
Python · Pandas · NumPy · Matplotlib · Seaborn
SciPy · Statsmodels · Lifelines (Kaplan-Meier)
Scikit-learn · XGBoost · SHAP
```

---

## 📬 Connect

**Aryan Gupta** — MSc Statistics  
[LinkedIn](https://linkedin.com/in/aryan-gupta-stats) · aryan.gupta.stats@gmail.com
