# Casino Analytics Pipeline

An end-to-end analytics pipeline that extends what a venue management platform already captures — turning operational CRM data into machine efficiency rankings, member behaviour analysis, and a weekly churn prediction model.

Built from first-hand operational knowledge as an Electronic Gaming Host, using synthetic data modelled from real platform outputs. The pipeline is production-ready: connecting it to a live export requires no new code.

---

## The Problem

Most venue management platforms record what happened — coin in, coin out, member visits, comp issuance. The gap is in what comes next.

Three questions the platform couldn't answer on its own:

- **Which machines are actually most profitable?** Volume rankings tell you which are busiest. They don't tell you which earn most efficiently per session.
- **Are the right members being retained?** A meaningful share of revenue on top-performing machines comes from players with no account attached — no way to follow up.
- **Who is about to stop coming?** Churn happens gradually. Without a predictive layer, there is no way to identify the pattern early enough to act.

---

## Pipeline Overview

```
01_generate_dataset.py      →   Synthetic 12-month dataset (6 tables, 117 machines, 3,100 members)
        ↓
02_floor_mix_analysis.py    →   Category-aware K-means machine efficiency ranking
        ↓
03_member_overlay.py        →   Member behaviour joined to machine tier results
        ↓
04_churn_model.py           →   Random Forest churn model, AUC 0.847, 3,100 members scored weekly
```

---

## Project Structure

```
casino-analytics-pipeline/
│
├── src/
│   ├── 01_generate_dataset.py       # Synthetic data generation
│   ├── 02_floor_mix_analysis.py     # Machine floor mix model (K-means)
│   ├── 03_member_overlay.py         # Member × machine tier overlay
│   └── 04_churn_model.py            # Churn prediction model
│
├── data/
│   └── synthetic/
│       └── GRV_NEON_Synthetic_Dataset.xlsx
│
├── outputs/
│   ├── GRV_Floor_Mix_v2_Category_Aware.xlsx
│   ├── GRV_Member_Overlay.xlsx
│   └── GRV_Churn_Model.xlsx
│
├── charts/                          # All model output charts (PNG)
├── presentation/
│   └── GRV_Analytics_Pitch.pptx    # 12-slide management presentation
│
├── requirements.txt
└── README.md
```

---

## How to Run

```bash
git clone https://github.com/CheedTriad/casino-analytics-pipeline
cd casino-analytics-pipeline
pip install -r requirements.txt

# Run in order
python src/01_generate_dataset.py
python src/02_floor_mix_analysis.py
python src/03_member_overlay.py
python src/04_churn_model.py
```

Each script reads from `data/synthetic/` and writes outputs to `outputs/` and `charts/`.

---

## Key Findings (Synthetic Data)

| Finding | Value |
|---|---|
| Slots in Review tier | 40% |
| Novomatic win vs all other manufacturers | 3× |
| Anonymous revenue on Elite & Strong machines | £1.45M |
| Revenue at risk — Critical + High churn members (3-month) | £1.45M |
| Churn model AUC (Random Forest) | 0.847 |
| Members in Critical churn tier | 339 |

---

## Sample Charts

**Machine Efficiency by Category**
![Efficiency by Category](charts/c1_efficiency_by_category.png)

**NEON Coin In Rank vs Efficiency Rank — Top 30 Slots**
![Rank Comparison](charts/c5_rank_comparison_slots.png)

**Tracked vs Anonymous Revenue by Machine Tier**
![Tracked vs Anonymous](charts/mo_c1_tracked_vs_anon.png)

**Churn Risk Distribution — 3,100 Members**
![Churn Risk](charts/ch_c4_risk_distribution.png)

**Feature Importance (SHAP) — Churn Model**
![SHAP Importance](charts/ch_c2_shap_importance.png)

---

## Model Performance

| Model | AUC | F1 | Precision | Recall |
|---|---|---|---|---|
| Logistic Regression | 0.820 | 0.594 | 0.701 | 0.516 |
| **Random Forest** | **0.847** | **0.651** | 0.651 | 0.651 |
| XGBoost | 0.830 | 0.577 | 0.647 | 0.522 |

### Note on methodology

Temporal features (days since last visit, visit trend, recent visit counts) are generated using a behaviour-first simulation — visit patterns are modelled via random walk on monthly rates, and churn labels are derived probabilistically from those patterns. This prevents data leakage and produces realistic class overlap. AUC of 0.847 reflects genuine model learning, not inflated performance from label-correlated inputs.

---

## Tech Stack

Python · pandas · NumPy · scikit-learn · XGBoost · SHAP · imbalanced-learn (SMOTE) · matplotlib · openpyxl

---

## Data Note

All data in this repository is synthetic. It was modelled from anonymised structural outputs of a real venue management platform and calibrated to match realistic operational ranges. No real customer data, no proprietary system data, and no identifiable information is included anywhere in this repository.

---

## Author

**Chidiere Oluoma**
Final-year BSc Computer Science, De Montfort University
[LinkedIn](https://www.linkedin.com/in/chidiere-oluoma/) · [GitHub](https://github.com/CheedTriad)
