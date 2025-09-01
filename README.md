# Advanced Cyberattack Detection by Integrating Clustering into a Single Supervised Model

> A reproducible pipeline that enriches a supervised IDS classifier with **unsupervised clustering signals** (K‑Means, GMM, HDBSCAN) on the **CSE‑CIC‑IDS2018** dataset, then explains decisions with **SHAP**.

---

## 📌 TL;DR
- **Goal**: Improve intrusion detection by adding **behavioral clusters** as features to a **single ensemble classifier** (Random Forest + Gradient Boosting + XGBoost via soft voting).
- **Result**: Baseline already achieves ~**0.999–1.000** Accuracy/Precision/Recall/F1 on sampled data; **adding clusters does not significantly improve** performance in this setting.
- **Explainability**: SHAP indicates traditional flow features dominate; cluster features contribute marginally.

---

## ✨ Features
- Data ingestion, cleaning, de‑duplication, **NaN/Inf handling**, type coercion
- **Clustering**: K‑Means, Gaussian Mixture (GMM), **HDBSCAN**
- **Visualization**: PCA / t‑SNE cluster plots; silhouette scores
- **Modeling**: VotingClassifier(**RF+GB+XGB**), stratified train/test
- **Augmented features**: cluster id (numeric) and **one‑hot** encodings
- **Explainability**: **SHAP** global/feature importance and comparison of cluster vs. original features

---

## 🗂️ Project Artifact
- Main notebook/script: `agoujil_mouhcine_ellaili_daoud_abiba_achraf_aouragh_chaimaa_zitouni_salah_eddine_notebook.py` (exported from Colab).

> The code expects a CSV named like `02-14-2018.csv` (CSE‑CIC‑IDS2018 slice). Update the path variables inside the script if your file lives elsewhere.

---

## 🧰 Environment
Tested with:
- **Python** 3.10+
- **NumPy 1.24.4**, **SciPy 1.10.1**, **scikit‑learn 1.2.2**
- **xgboost**, **hdbscan**, **pandas**, **matplotlib**, **seaborn**, **shap**

### Quick setup
```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install --upgrade pip

# Pin core libs used inside the notebook
pip install numpy==1.24.4 scipy==1.10.1 scikit-learn==1.2.2

# Rest of the stack
pip install pandas xgboost hdbscan shap matplotlib seaborn jupyter
```

> **Note (Linux/Windows):** `hdbscan` may need build tools. If install fails on Windows, install *Microsoft C++ Build Tools*. On Linux, ensure Python headers (e.g., `python3-dev`) are present.

---

## 📥 Data
- **Dataset**: CSE‑CIC‑IDS2018 (Kaggle). Download the relevant CSV(s) locally.
- **Path used in code**: `02-14-2018.csv`. Change the path near file reads if needed.

---

## 🚀 How to Run

### Option A — Jupyter / Colab
1. Open the notebook/script in Jupyter or upload to **Google Colab**.
2. Upload `02-14-2018.csv` to the runtime or mount your drive.
3. Run cells top‑to‑bottom.

### Option B — Run the `.py` as a script
```bash
python agoujil_mouhcine_ellaili_daoud_abiba_achraf_aouragh_chaimaa_zitouni_salah_eddine_notebook.py
```
If you hit memory/time limits, reduce the **sampling fraction** (code uses ~**6%** of rows for heavy steps) or disable some plots.

---

## 🧪 Pipeline Overview
1. **Load & sample**: read CSV, optionally sample ~6% for speed.  
2. **Clean**: drop duplicates; replace `±inf→NaN→mean/0`; coerce numerics.  
3. **Target**: use `Label` (multi‑class); also create numeric `Target` for modeling.  
4. **Split**: stratified train/test.  
5. **Clustering** (on numeric features):  
   - **K‑Means** (k found via elbow ≈ 3–10),  
   - **GMM**,  
   - **HDBSCAN** (no k, handles variable density).  
6. **Add cluster features**: numeric id + **one‑hot**.  
7. **Model**: VotingClassifier of **RF, GB, XGB** (soft voting).  
8. **Metrics**: Accuracy/Precision/Recall/F1; confusion matrix.  
9. **Explainability**: **SHAP** summary + top features plots.  
10. **Compare**: baseline vs. with clusters (numeric / one‑hot).  
(See inline comments and printouts in the code.)

---

## 📊 Key Results (summarized)
- **Baseline (no clusters)**: ≈ **0.999–1.000** across Accuracy/Precision/Recall/F1 on sampled data.  
- **+ Clusters (numeric / one‑hot)**: **no significant improvement**; performances remain ≈ baseline.  
- **SHAP**: **Dst Port**, header/flow statistics rank highest; **cluster features** show limited marginal importance.

> Interpretation: With this dataset slice and preprocessing, the supervised ensemble is already very strong; clustering‑derived signals don’t add measurable lift.

---

## 🧪 Repro Tips
- Tune `n_clusters` (K‑Means/GMM), `min_cluster_size` (HDBSCAN), and **sampling** to balance quality vs. runtime.
- If memory is tight, drop high‑cardinality / quasi‑constant columns and keep the most informative features.
- For a fuller benchmark, run multiple days of CSE‑CIC‑IDS2018 and macro‑average metrics.

---

## 📁 Suggested Repo Structure
```
.
├─ README.md
├─ requirements.txt
├─ agoujil_mouhcine_ellaili_daoud_abiba_achraf_aouragh_chaimaa_zitouni_salah_eddine_notebook.py
├─ data/
│  └─ 02-14-2018.csv
└─ reports/
   ├─ figures/   # PCA / t‑SNE / SHAP plots
   └─ logs/
```
Create `requirements.txt` from your env when ready:
```bash
pip freeze > requirements.txt
```

---

## 👥 Authors
AGOUJIL **Mouhcine**, ELLAILI **Daoud**, ABIBA **Achraf**, AOURAGH **Chaimaa**, ZITOUNI **Salah‑eddine**.
