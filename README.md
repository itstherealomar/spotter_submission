# Spotter Machine Learning Engineer Assessment — Freight Rate Prediction

**Author:** Omar Abourida  
**Role:** Machine Learning Engineer Assessment

---

## Video Presentation
- **Loom Walkthrough (2–3 minutes)**: [Watch the Loom Video Walkthrough Here](https://www.loom.com/share/36bf9d32856f471985d151bcb78bcc29) 

---

## 📋 Deliverables Overview

This repository contains all official deliverables requested in `freight-rate-ml-assessment.pdf`:

| Deliverable | File / Path | Description |
|---|---|---|
| **Solution Notebook** | [`freight_rate_prediction.ipynb`](freight_rate_prediction.ipynb) | Complete, pre-executed, self-contained notebook (EDA, cleaning, lean feature engineering, model benchmarks, full retraining, and submission generation). |
| **Validation Predictions** | [`validation_predictions.csv`](validation_predictions.csv) | Final predictions for all 12,000 loads matching `validation-predictions-template.csv` format (`load_id,predicted_rate`). |
| **December Scenario** | [`december-chart-inputs.csv`](december-chart-inputs.csv) | Completed 31-day rate predictions for the fixed Lexington $\to$ Fort Wayne lane. |
| **Scorer Chart** | [`scorer_results/candidate_december.png`](scorer_results/candidate_december.png) | Time-series forecast chart generated and validated by Spotter's `score.py`. |
| **Technical Report** | [`freight_rate_prediction_report.pdf`](freight_rate_prediction_report.pdf) | Publication-grade report covering validation strategy, data cleaning, feature engineering, model benchmarks, and the December scenario. |
| **Environment Dependencies** | [`requirements.txt`](requirements.txt) | Minimal, pinned dependencies to ensure 100% reproducibility. |

---

## Model Benchmark (Out-of-Time Temporal Split)

Validation was conducted using a strict **Out-of-Time (OOT) Forward Split** (Training: Jan 1 – Aug 31, 2025; Evaluation: Sep 1 – Oct 31, 2025) to eliminate lookahead data leakage:

| Model Architecture | MAE ($) | RMSE ($) | MAPE (%) | $R^2$ |
| :--- | :---: | :---: | :---: | :---: |
| **Naive Baseline** (`dist * quote`) | $193.20 | $347.76 | 9.90% | 0.9362 |
| **Ridge Regression** | $93.08 | $134.90 | 5.45% | 0.9904 |
| **Random Forest Regressor** | $76.29 | $123.61 | 3.22% | 0.9919 |
| **XGBoost Regressor (Champion)** | **$64.45** | **$110.29** | **2.72%** | **0.9936** |

---

## ⚙️ Quickstart & Reproduction Instructions

### 1. Clone & Set Up Environment

```bash
# Clone the repository
git clone <your-repo-url>
cd spotter_submission

# Create and activate virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate    # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Run the End-to-End Notebook

The entire workflow is automated in `freight_rate_prediction.ipynb`. It is already pre-executed with all cell outputs, tables, and plots visible.

To re-run sequentially from scratch:
```bash
jupyter notebook freight_rate_prediction.ipynb
```
Or execute headlessly from the command line:
```bash
python -m nbconvert --to notebook --execute --inplace freight_rate_prediction.ipynb
```

### 3. Verify Submission Files with `score.py`

Run the official validation script to verify schema compliance and regenerate the December forecast chart:

```bash
python score.py --predictions validation_predictions.csv --december-predictions december-chart-inputs.csv
```

Expected verification output:
```
Validated 12,000 final predictions.
Validated 31 fixed December predictions.
Created chart: scorer_results/candidate_december.png
Final validation metrics are calculated by Spotter after submission.
```