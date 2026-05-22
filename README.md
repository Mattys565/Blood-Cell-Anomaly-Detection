# Blood Cell Anomaly Detection

Exploration of machine learning methods for detecting and classifying blood cell anomalies from tabular morphological features.

---

## Context

Blood smear analysis is a routine but time-consuming task in hematology. The goal of this project was to see how far you can get with tabular features alone — no images, just morphological measurements, color descriptors, and clinical CBC values — before needing to reach for a full computer vision pipeline.

---

## Dataset

36 features across 4 categories:

| Category | Features |
|---|---|
| Morphology | diameter, circularity, eccentricity, lobularity, nucleus area, chromatin density, granularity |
| Color | mean RGB, stain intensity (Wright-Giemsa) |
| Clinical CBC | WBC count, hemoglobin, hematocrit, MCV, MCHC, platelet count |
| Acquisition | microscope model, staining protocol, magnification, resolution |

19 cell types total — 7 normal, 12 abnormal spanning leukemia, anemia, sickle cell, infection, and artefact categories.

**Normal (7):** Neutrophil, Lymphocyte, Monocyte, Eosinophil, Basophil, Normal RBC, Platelet

**Abnormal (12):**
- 🔴 Leukemia — Blast Cell, Prolymphocyte
- 🟠 Anemia — Elliptocyte, Schistocyte, Spherocyte, Target Cell
- 🟡 Sickle Cell — Sickle Cell
- 🟣 Infection — Hypersegmented Neutrophil, Toxic Granulation, Reactive Lymphocyte
- ⚫ Artefact — Smudge Cell, Artefact

---

## Structure

The analysis is split into four parts:

**I. EDA** — distributions, class balance, correlation with the target variable

**II. Binary classification** — Normal vs Anomaly, with permutation importance and SHAP analysis across three models (Random Forest, LightGBM, XGBoost)

**III. Disease category classification** — 8-class multiclass (Normal, Leukemia, Anemia, Sickle Cell, Infection, Artefact…), with per-class SHAP summary plots

**IV. Cell type classification** — 19-class multiclass, with focused SHAP analysis on the cell types the model struggles with most

---

## Results

XGBoost was the best performing model across all three tasks. A few highlights:

- Leukemia recall of **1.00** in the disease category task — no false negatives on the most critical class
- The errors in cell type classification follow the actual difficulty of hematology: Blast Cell vs Prolymphocyte, subtle RBC morphology, ambiguous granulation patterns
- SHAP confirmed that the most influential features (lobularity, nucleus area, chromatin density, granularity) align with what hematologists actually look at

---

## Key Finding

The model doesn't just perform well — it fails in the right places. Confusions happen between cell types that are genuinely hard to distinguish, even for trained eyes. That's a reasonable signal that something real is being learned from the morphological features.

---

## Stack

```
Python · XGBoost · LightGBM · scikit-learn · SHAP · pandas · seaborn
```
