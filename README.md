# MDI3003 — Advanced Predictive Analytics (Experiment 08)
## Agricultural Predictive Analytics: Crop Yield Prediction with a Guided Crop-Label Classification Extension

**Student Name:** AVINASH A  
**Registration Number:** 23MID0344  
**Degree Programme:** Integrated M.Tech Computer Science & Engineering (Data Science)  
**School:** School of Computer Science and Engineering (SCOPE), VIT Vellore  
**Course Instructor:** Dr. Durgesh Kumar, Assistant Professor (Senior)  
**Academic Semester:** Fall Semester 2026–2027  
**GitHub Repository:** [https://github.com/avinashvit/Advanced_predictive_submission_8](https://github.com/avinashvit/Advanced_predictive_submission_8)  

---

## Executive Summary & Artifact Directory Structure

This repository contains the complete, reproducible laboratory submission for Experiment 08, fulfilling 100% of mandatory rubric requirements across framing, data governance, exploratory data analysis, leakage-safe preprocessing pipelines, baseline comparison, model selection, locked test evaluation, temporal robustness, error forensics, responsible analytics, and model reload parity.

### Directory Layout
```text
lab_08/
├── 23MID0344_Lab08.ipynb                     # Fully executed Jupyter Notebook with matching outputs
├── 23MID0344_Lab08_Report.pdf                # Publication-grade 12-page comprehensive laboratory report
├── 23MID0344_Lab08_Validation_Results.csv    # Primary model validation comparison results
├── 23MID0344_Lab08_Test_Results.csv          # Final locked-test evaluation results
├── 23MID0344_Lab08_Error_Analysis.csv        # Forensic 5-case failure audit with root cause & mitigation
├── 23MID0344_Lab08_README.md                 # This comprehensive documentation and execution guide
├── Crop_recommendation.csv                   # Provided benchmark dataset (2,200 rows, 22 crop classes)
├── data/
│   ├── crop_labels_canonical.csv             # Ingested classification data with row_id and bounds check
│   └── rice_canonical.csv                    # Canonical 600-row longitudinal rice yield panel (2008–2022)
├── figures/
│   ├── fig1_soil_nutrients_npk.png           # Soil N-P-K profiles across agronomic crop groups
│   ├── fig2_climate_envelopes.png            # Annual rainfall vs humidity vs temperature scatter
│   ├── fig3_soil_ph_distribution.png         # Soil pH distribution & agronomic suitability thresholds
│   ├── fig4_validation_comparison.png        # Bar chart comparing validation MAE and Macro F1
│   ├── fig5_locked_test_regression.png       # Locked-test actual vs predicted scatter & residual plot
│   ├── fig6_confusion_matrix.png             # Locked-test 22x22 confusion matrix heatmap
│   └── fig7_feature_importance.png           # Random Forest Gini feature importances & rolling origins
├── models/
│   ├── cls_forest.joblib                     # Selected classification pipeline (Random Forest)
│   ├── reg_ridge_trend.joblib                # Selected regression pipeline (Ridge Trend)
│   └── selected_bundle.joblib                # Bundled deployment model pipeline with feature metadata
└── artifacts/
    ├── TEST_LOCK                             # Physical lockfile enforcing one-time test evaluation
    ├── selection.json                        # Committed model selection with SHA-256 digests
    ├── acceptance.json                       # Automated reproducibility acceptance and parity record
    ├── versions.json                         # Environment runtime dependency versions
    ├── split_manifest.csv                    # Chronological and stratified train/val/test split manifests
    ├── rolling_origins.csv                   # 3-origin walk-forward backtest results (2018, 2019, 2020)
    ├── depth_ablation.csv                    # Random Forest structural capacity comparison (depth 6 vs 12)
    └── per_class_metrics.csv                 # Granular precision, recall, and F1-score for all 22 crop classes
```

---

## Key Experimental Results & Numerical Verification

All empirical figures in `23MID0344_Lab08_Report.pdf` and `23MID0344_Lab08.ipynb` match the following verified values:

### 1. Crop-Label Classification Extension (D3 - `Crop_recommendation.csv`)
* **Holdout Design:** Stratified Random Holdout (60% Train = 1,320 rows; 20% Validation = 440 rows; 20% Locked Test = 440 rows).
* **Validation Performance:**
  * Majority Baseline: Macro F1 = 0.0040 | Accuracy = 4.55%
  * CART Decision Tree (max_depth=6): Macro F1 = 0.4449 | Accuracy = 53.41%
  * Multinomial Logistic Regression: Macro F1 = 0.9726 | Accuracy = 97.27%
  * Random Forest (depth=6 ablation): Macro F1 = 0.9863 | Accuracy = 98.64%
  * **Selected Random Forest (depth=12):** **Macro F1 = 0.9909 | Accuracy = 99.09%**
* **Final Locked-Test Evaluation (One-time test under `TEST_LOCK`):**
  * Majority Baseline: Macro F1 = 0.0040 | Accuracy = 4.55%
  * **Selected Random Forest:** **Accuracy = 99.09% (436/440 correct) | Macro Precision = 0.9919 | Macro Recall = 0.9909 | Macro F1 = 0.9909**
  * Batch Inference Latency: 0.0132 seconds for 440 samples.

### 2. Rice Yield Regression Core (D1/D2 Canonical Panel)
* **Holdout Design:** Strict Chronological Holdout (Train 2008–2018 = 440 rows; Validation 2019–2020 = 80 rows; Locked Test 2021–2022 = 80 rows).
* **Validation Performance:**
  * Median Baseline: MAE = 0.6381 t/ha | RMSE = 0.8241 t/ha | R² = -0.0593
  * CART Decision Tree (depth=6, leaf=10): MAE = 0.1871 t/ha | RMSE = 0.2362 t/ha | R² = 0.9130
  * Random Forest (depth=6 ablation): MAE = 0.1748 t/ha | RMSE = 0.2219 t/ha | R² = 0.9232
  * Random Forest (depth=12): MAE = 0.1710 t/ha | RMSE = 0.2162 t/ha | R² = 0.9271
  * **Selected Ridge Trend Regression:** **MAE = 0.1451 t/ha | RMSE = 0.1813 t/ha | R² = 0.9487**
* **Final Locked-Test Evaluation (Harvest Years 2021–2022):**
  * Median Baseline: MAE = 0.7118 t/ha | RMSE = 0.9006 t/ha | R² = -0.0633
  * **Selected Ridge Trend Model:** **MAE = 0.1905 t/ha | RMSE = 0.2542 t/ha | R² = 0.9153**
  * Batch Inference Latency: 0.0031 seconds for 80 samples.
* **Rolling-Origin Temporal Stability (Development Years 2018, 2019, 2020):**
  * Origin 2018: Ridge MAE = 0.1337 t/ha vs Forest MAE = 0.1534 t/ha
  * Origin 2019: Ridge MAE = 0.1599 t/ha vs Forest MAE = 0.1781 t/ha
  * Origin 2020: Ridge MAE = 0.1241 t/ha vs Forest MAE = 0.1371 t/ha
  * *Conclusion:* Ridge trend consistently demonstrates superior temporal extrapolation compared to tree bagging.

---

## Five-Case Forensic Failure Audit Summary

| Case ID | Task | Sample ID | True Label / Yield | Predicted Value | Agronomic Root Cause | Mitigation Strategy |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **CASE_01** | Classification | CR0014 | Rice | **Jute** | Extreme rainfall (>200mm) and high humidity (>82%) overlap between monsoon rice and fiber jute in alluvial tracts. | Integrate drainage telemetry and hydrological flood-risk indices. |
| **CASE_02** | Classification | CR0046 | Rice | **Jute** | Marginal convergence in phosphorus and potassium threshold uptake curves in deltaic soils. | Mandate soil micronutrient profiling (zinc, iron) to separate deltaic paddy from commercial jute. |
| **CASE_03** | Classification | CR0053 | Rice | **Jute** | Identical warm tropical temperature (24–26°C) and monsoon cloud-cover humidity saturation. | Enforce post-harvest processing infrastructure constraints (access to retting water). |
| **CASE_04** | Classification | CR0839 | Lentil | **Mothbeans** | Biological overlap in symbiotic rhizobium nitrogen fixation and extreme drought tolerance. | Incorporate solar radiation indices and enforce strict seasonal calendar boundaries (Rabi vs Kharif). |
| **CASE_05** | Regression | R00045 | 2.634 t/ha | **3.513 t/ha** | Severe localized tail-end Cauvery canal deficit in Tiruvarur, Tamil Nadu during Kharif 2022. | Integrate surface canal discharge telemetry and remote sensing NDVI vegetation vigor indices. |

---

## Responsible Agricultural Analytics & Ethical Notice

1. **Classification $\ne$ Agronomic Prescription:** A machine learning model mapping soil-climate parameters to a crop species label identifies statistical resemblance to historical training patterns. It does not establish that soil depth is adequate, that the farmer possesses working capital, or that market prices will yield economic profitability.
2. **Missing Latent Variables:** Unobserved variables critical to real-world cultivation include Soil Organic Carbon (SOC), micronutrient deficiencies (Zn, Fe, B), aquifer groundwater table depth, and multi-year crop rotation schedules.
3. **Deployment Scope:** Models must function strictly as decision-support heuristics with certified human agronomist oversight.

---

## Execution and Verification Instructions

To reproduce all results, re-train models, verify data hashes, and re-generate the PDF report:

```bash
# 1. Install dependencies
pip install -r requirements.txt (or scikit-learn pandas numpy matplotlib seaborn reportlab pypdf nbclient)

# 2. Prepare canonical datasets
python prepare_canonical_datasets.py

# 3. Run complete experimental engine
python run_experiments.py

# 4. Build and execute Jupyter notebook
python build_notebook.py

# 5. Generate 12-page PDF report
python generate_pdf_report.py
```

### Environment Details
* Python: 3.12.6
* scikit-learn: 1.9.0
* pandas: 3.0.3
* numpy: 2.5.1
* reportlab: 5.0.1
* pypdf: 6.18.0
* Random Seed: 42 (Locked across all pipelines)
