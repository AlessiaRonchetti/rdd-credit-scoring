# Do Algorithms Amplify Financial Inequality?
## Evidence from Causal Inference and Machine Learning in FinTech Lending

**Author:** Ronchetti Alessia — 260985  
**Course:** Computational Social Science — A.A. 2025–2026  
**Programme:** MSc in Data Science, University of Trento  

---

## Overview

This project.zip contains the code, replication instructions (readme), and final paper. The project combines two complementary analyses on LendingClub peer-to-peer lending data (2007–2018):

- **Part 1 — Causal Inference:** A Fuzzy Regression Discontinuity Design (IV2SLS) exploiting the FICO 740 threshold as an instrument for interest rates, estimating the causal effect on default probability.
- **Part 2 — Algorithmic Fairness Audit: Three ML classifiers (Logistic Regression, Random Forest, XGBoost) evaluated on standard fairness metrics (Demographic Parity, Equal Opportunity, Predictive Parity), SHAP feature attribution, and a conditional within-FICO-quintile bias analysis.

---

## Repository Structure

```
rdd-credit-scoring/
├── Report_CSS_Ronchetti_Alessia.pdf    # Final paper
├── RDD_code.ipynb                      # Single notebook reproducing all results
└── README.md                           # This file
```

---

## Data Access

### Raw data
The raw dataset (`accepted_2007_to_2018Q4.csv`) is the publicly available LendingClub loan dataset (2007–2018), hosted on Kaggle. Not included in this repository due to its size (~1.5 GB).

**Download it here:**  
[https://www.kaggle.com/datasets/wordsforthewise/lending-club](https://www.kaggle.com/datasets/wordsforthewise/lending-club)

> **Note:** A free Kaggle account is required. Once logged in, click **Download** on the dataset page and extract `accepted_2007_to_2018Q4.csv` from the zip archive.

**Setup steps:**

1. Download `accepted_2007_to_2018Q4.csv` from Kaggle
2. Upload it to your Google Drive, inside a folder called `Progetto_RDD/`  
   (e.g. `MyDrive/Progetto_RDD/accepted_2007_to_2018Q4.csv`)
3. Open `RDD_code.ipynb` in Google Colab
4. When prompted, authorize Colab to access your Drive
5. Update the **three path variables** at the top of the notebook (Section 2) if your folder structure differs:

```python
PATH_ORIGINAL = "/content/drive/MyDrive/Progetto_RDD/accepted_2007_to_2018Q4.csv"
PATH_CLEAN    = "/content/drive/MyDrive/Progetto_RDD/lending_clean_small.csv"
OUTPUT_DIR    = "/content/drive/MyDrive/Progetto_Fairness/"
```

> **Important:** Part 1 figures and tables are saved to paths **hardcoded** as `/content/drive/MyDrive/Progetto_RDD/` throughout the notebook (not via `PATH_ORIGINAL`). If you rename or move the `Progetto_RDD/` folder, search for that string inside the notebook and update it accordingly. Part 2 outputs use `OUTPUT_DIR` and can be redirected by changing only that variable.

### Preprocessed cache (`lending_clean_small.csv`)
On the **first run**, the notebook automatically generates a cleaned and filtered version of the raw data saved as `lending_clean_small.csv` at the location defined by `PATH_CLEAN`. The preprocessing steps applied are:

1. **Variable selection:** only four columns are retained — `fico_range_low`, `int_rate`, `loan_status`, and `annual_inc`.
2. **Resolved loans only:** observations still active at extraction time are dropped; only loans with a final outcome (`Fully Paid`, `Charged Off`, or `Default`) are kept.
3. **Binary outcome:** a `default` variable is constructed (1 = Charged Off or Default; 0 = Fully Paid).
4. **Missing values:** rows with any missing value on the retained variables are dropped.
5. **Income transformation:** `annual_inc` is log-transformed (`log_annual_inc`) to address right skew; borrowers are split into `Low Income` / `High Income` at the sample median.

On **subsequent runs**, the notebook loads `lending_clean_small.csv` directly, skipping the heavy preprocessing step.

---

## Dependencies

Install all required packages before running the notebook:

```bash
pip install linearmodels shap xgboost scikit-learn pandas numpy matplotlib seaborn statsmodels
```

In Colab, the notebook installs `linearmodels`, `shap`, and `xgboost` automatically via `!pip install` at the top of the first cell. The remaining packages (`pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `statsmodels`) are pre-installed in the standard Colab environment.

---

## How to Reproduce

1. Open `RDD_code.ipynb` in **Google Colab** (recommended) or Jupyter Notebook.
2. Mount Google Drive if using Colab and update the three path variables as described above.
3. Run all (**Runtime → Run all**).

**First run only:** the notebook loads the raw dataset, filters for resolved loans, and saves a lightweight cached file (`lending_clean_small.csv`). This takes a few minutes.  
**Subsequent runs:** the cached file is loaded directly, skipping the heavy preprocessing step.

The notebook is structured in two self-contained parts:


- **Sections 0-4:** Dependencies, imports, data loading/preprocessing, and feature engineering shared by both parts.
- **Sections 5–15 (Part 1):** Fuzzy RDD causal analysis — figures and tables for §4.2, §5.1, §6.1 of the paper, saved to `Progetto_RDD/`.
- **Sections 16–24 (Part 2):** Algorithmic fairness audit — figures and tables for §4.3, §5.2–5.4, §6.2 of the paper, saved to `Progetto_Fairness/`.

---

## Expected Outputs

All outputs are saved automatically. Part 1 figures/tables go to the `Progetto_RDD/` folder; Part 2 figures/tables go to the `Progetto_Fairness/` folder.

| Output file | Description | Figure/Table in paper |
|---|---|---|
| `fig1_global_exploration.pdf` | Interest rate discontinuity across full FICO range | Figure 1 (p. 4) |
| `fig2_density_test_740.pdf` | McCrary density test at FICO 740 | Figure 2 (p. 12) |
| `fig3_first_stage_plot.pdf` | First-stage discontinuity in interest rate | Figure 3 (p. 13) |
| `fig4_outcome_plot.pdf` | Reduced-form default probability at cutoff | Figure 4 (p. 14) |
| `fig5_balance_check.pdf` | Covariate balance — log(income) at FICO 740 | Figure 12 (p. 21) |
| `table1_summary_stats.csv` | Descriptive statistics at FICO 740 cutoff | Table 1 (p. 9) |
| `fig1_roc_by_group.pdf` | ROC curves by income group (3 models) | Figure 5 (p. 15) |
| `fig2_fairness_bars.pdf` | DP, EO, PP metrics by model and income group | Figure 6 (p. 16) |
| `fig3_shap_global.pdf` | Global mean \|SHAP\| bar chart (XGBoost) | Figure 7 (p. 17) |
| `fig4_shap_beeswarm.pdf` | SHAP beeswarm — directional contributions | Figure 8 (p. 17) |
| `fig5_shap_by_group.pdf` | Mean \|SHAP\| by income group (XGBoost) | Figure 9 (p. 18) |
| `fig6_conditional_fairness.pdf` | Predicted vs actual default by FICO quintile | Figure 10 (p. 19) |
| `fig7_excess_bias.pdf` | Excess bias (predicted gap − actual gap) by quintile | Figure 11 (p. 20) |
| `table3_fairness_metrics.csv` | Detailed fairness metrics by model and group | Table 3 (p. 16) |
| `table4_shap_by_group.csv` | Mean \|SHAP\| by feature and income group | Table 4 (p. 18) |
| `table5_conditional_fairness.csv` | Predicted and actual default rates by FICO bin | Table 5 (p. 19) |
| `table6_bias_gaps.csv` | Excess bias values by FICO quintile | Table 6 (p. 20) |
| `table_summary_fairness.csv` | Fairness gaps summary across all models | Table 2 (p. 15) |


---

## License

MIT
