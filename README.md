# credit-risk-model
# Credit Risk Model — Bati Bank

## Overview
End-to-end credit risk scoring system for Bati Bank's Buy Now Pay Later service, built using transaction data from the eCommerce partner platform.

## Credit Scoring Business Understanding

### 1. How does Basel II influence the need for interpretable models?
The Basel II Capital Accord requires banks to hold capital reserves proportional to their credit risk exposure. This means every risk prediction must be:
- **Auditable** — regulators must be able to inspect model logic
- **Documented** — every modeling choice must be justified in writing
- **Interpretable** — a loan officer must be able to explain WHY a customer was rejected

This rules out pure black-box models. A Logistic Regression with Weight of Evidence (WoE) transformations satisfies Basel II because each coefficient has a clear business interpretation. A Random Forest may perform better but its predictions are harder to defend to regulators.

### 2. Why is a proxy variable necessary and what are its risks?
The raw dataset contains **no default label** — we don't know which customers actually failed to repay loans because this is eCommerce transaction data, not loan repayment data.

A **proxy variable** is a substitute: we use behavioral patterns (RFM — Recency, Frequency, Monetary) to estimate which customers are likely to be high risk. Customers who:
- Haven't transacted recently (low Recency)
- Transact infrequently (low Frequency)
- Spend very little (low Monetary)

...are labeled as **high risk** proxies.

**Business risks of proxy-based prediction:**
- The proxy may not perfectly correlate with actual default
- We may unfairly label low-income but reliable customers as high risk
- Model must be monitored and recalibrated as real default data becomes available

### 3. Trade-offs: Interpretable vs High-Performance Models

| Factor | Logistic Regression + WoE | Gradient Boosting (XGBoost) |
|--------|--------------------------|----------------------------|
| Interpretability |  High — coefficients are meaningful |  Low — black box |
| Performance | Moderate | High |
| Regulatory compliance |  Easy to audit |  Hard to defend |
| Scorecard conversion Direct Complex |
| Monitoring | Simple | Complex |

**Recommendation:** Use Logistic Regression as the primary model for regulatory compliance. Use XGBoost as a challenger model to measure performance uplift.

## Project Structure

credit-risk-model/
├── .github/workflows/ci.yml
├── data/raw/                  # Raw data (gitignored)
├── data/processed/            # Processed data (gitignored)
├── notebooks/eda.ipynb        # Exploratory analysis
├── src/
│   ├── data_processing.py
│   ├── train.py
│   ├── predict.py
│   └── api/
│       ├── main.py
│       └── pydantic_models.py
├── tests/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── README.md

