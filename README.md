# 💧 Tanzania Water Wells — Pump Functionality Prediction

> Predicting the operational status of water points across Tanzania to guide maintenance, funding, and infrastructure investment.

---

## Table of Contents

- [Problem Statement](#problem-statement)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
- [How to Run](#how-to-run)
- [Recommendations](#recommendations)
- [Future Work](#future-work)

---

## Problem Statement

Tanzania is East Africa's largest country, home to nearly **60 million people** — yet:

- **25 million** lack access to clean water
- **40 million** lack access to improved sanitation
- Only **30.6%** of households use recommended water treatment methods
- Poor sanitation contributes to an estimated **432,000 diarrhea-related deaths per year** (WHO, 2019)

The Tanzanian government has invested heavily in water well infrastructure, but many existing wells are **non-functional or in need of repair**, limiting their impact. Identifying which pumps need attention — before full failure — is critical to improving public health outcomes.

**Goal:** Build a classification model that predicts the operational status of water points as one of three classes:

| Label | Description |
|---|---|
| `functional` | Operating correctly |
| `functional needs repair` | Working but requires maintenance |
| `non functional` | Not operational |

---

## Dataset

**Source:** [DrivenData — Pump it Up: Data Mining the Water Table](https://www.drivendata.org/competitions/7/)

| Property | Value |
|---|---|
| Total records | 59,400 water points |
| Features | 40 (11 numeric, 29 categorical) |
| Target | `status_group` (3 classes) |

### Key Features Used

- **Geographic:** `latitude`, `longitude`, `altitude`, `region`
- **Infrastructure:** `extraction_type`, `waterpoint_type`, `source`, `quantity`
- **Management:** `funder`, `installer`, `management`, `payment`
- **Temporal:** `construction_year`, `date_recorded`

### Data Cleaning Summary

- Removed **23 redundant or duplicate columns** (e.g. overlapping region/district fields)
- Addressed missing values in `population`, `construction_year`, and `gps_height`
- Applied feature engineering and encoding for categorical variables
- Applied standard scaling to numeric features

---

## Methodology

```
Raw Data
   │
   ▼
Data Cleaning & Feature Engineering
   │
   ▼
Exploratory Data Analysis (EDA)
   │
   ▼
Binary Classification (Baseline)
   │
   ▼
Ternary Classification (Final Model)
   │
   ▼
Evaluation & Recommendations
```

### EDA Highlights

- **Submersible pumps** have the highest water availability, typically located at lower altitudes closer to the water table
- **Communal standpipes** are the most common waterpoint type across Tanzania
- Wells with **no payment scheme** are disproportionately non-functional
- The **Government of Tanzania** is the largest funder and second-largest installer of water points

### Models Evaluated

| Model | Notes |
|---|---|
| Logistic Regression | Best performer — interpretable baseline |
| Decision Tree | Explored for ternary classification |
| Additional classifiers | Evaluated during experimentation |

---

## Results

| Metric | Score |
|---|---|
| **Best Model** | Logistic Regression |
| **Test Accuracy** | 62% |
| **Target Accuracy** | 70–75% |
| **Classification Type** | Ternary (3-class) |

The model correctly identifies functional, non-functional, and repair-needed wells above a naive baseline. Reaching the 70–75% target is the focus of ongoing improvement (see [Future Work](#future-work)).

---

## How to Run

### Prerequisites

```bash
Python >= 3.8
pip install -r requirements.txt
```

**Key dependencies:** `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `jupyter`

### Setup

```bash
# 1. Clone the repository
git clone https://github.com/your-username/Tanzania_water_wells_project.git
cd Tanzania_water_wells_project

# 2. Install dependencies
pip install -r requirements.txt

# 3. Add data files to the /data directory
#    Download from: https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/data/
#    Required files:
#      - training_set_values.csv
#      - training_set_labels.csv
#      - test_set_values.csv
```

### Run the Notebooks

```bash
jupyter notebook
```

| Notebook | Description |
|---|---|
| `01_data_understanding.ipynb` | Initial inspection, column analysis |
| `02_data_cleaning.ipynb` | Missing values, feature removal, encoding |
| `03_eda.ipynb` | Univariate & bivariate analysis, visualizations |
| `04_modeling.ipynb` | Model training, evaluation, comparison |

### Generate Predictions

```bash
python predict.py --input data/test_set_values.csv --output submissions/predictions.csv
```

---

## Recommendations

Based on model outputs and EDA findings:

1. **Audit government-funded wells** — the government is the largest funder, yet many of their wells show non-functional status; a targeted audit is warranted
2. **Prioritize unpaid wells for inspection** — no-payment wells are strongly correlated with non-functionality
3. **Focus repair efforts on lower-altitude regions** — submersible and motorpump wells at low altitude serve large populations
4. **Introduce payment or maintenance programs** — wells with payment schemes have significantly better functionality rates

---

## Future Work

- **SMOTE** to address class imbalance in the `functional needs repair` category
- **Advanced feature engineering** on high-cardinality categorical columns (`funder`, `installer`)
- **Ensemble methods** (Random Forest, XGBoost, Gradient Boosting) to push past the 70–75% accuracy target
- **Geospatial analysis** to identify regional clusters of failing infrastructure
- **Time-series features** from construction year and recording date to model well aging

---

## References

- Ministry of Health Tanzania (2019). *National Health Report*
- WHO (2019). *Sanitation and Health*
- Nsemwa, L. (2022). *Water Access Challenges in Tanzania*
- World Bank Open Data — Tanzania Population Statistics
- [DrivenData Competition: Pump it Up](https://www.drivendata.org/competitions/7/)

---

*Project by: Tanzania Water Wells Team | Data Source: DrivenData*
