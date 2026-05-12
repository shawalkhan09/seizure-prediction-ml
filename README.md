# Seizure Prediction: Preprocessing, Regularisation & Generalisation

**Course:** Machine Learning / Medical AI — Semester 6 Major Assignment  
**Author:** Shawal Khan

---

## Overview

This project investigates how preprocessing choices, model complexity, and regularisation strategies affect generalisation performance in epileptic seizure prediction. Using three EEG-based datasets of varying imbalance severity, two preprocessing pipelines are designed, overfitting and underfitting are demonstrated, and L1, L2, and Elastic Net regularisation are systematically compared under class-imbalanced conditions.

Primary metric: **PR-AUC** (Precision-Recall AUC), chosen due to class imbalance across all datasets.

---

## Research Questions

1. Does preprocessing **order** (scale-then-select vs select-then-scale) affect model performance?
2. Which regularisation method **generalises best** across diverse datasets?
3. Does Elastic Net consistently outperform L1 and L2?
4. How does **imbalance handling** interact with regularisation choice?

---

## Datasets

All three datasets are generated synthetically, reproducing the statistical properties of publicly known EEG benchmarks. No external downloads are required.

| # | Dataset | Samples | Features | Seizure % | Based On |
|---|---------|---------|----------|-----------|----------|
| 1 | UCI EEG Style | 11,500 | 178 | 20% | UCI ML Repository |
| 2 | CHB-MIT Style | 5,000 | 23 | 10% | CHB-MIT Clinical EEG |
| 3 | High-Imbalance | 8,000 | 50 | 5% | Clinical distribution |

---

## Preprocessing Pipelines

**Pipeline A — Filter-First**
```
Raw EEG → StandardScaler → VarianceThreshold → SelectKBest (f_classif) → LogisticRegression
```

**Pipeline B — Compress-First**
```
Raw EEG → StatFeatureExtractor → MinMaxScaler → PCA (95% variance) → LogisticRegression
```

---

## Experiments

- **Baseline** — Both pipelines evaluated across all 3 datasets
- **Overfitting / Underfitting** — C swept from 0.0001 to 10000; learning curves plotted
- **Regularisation Study** — L1 (Lasso), L2 (Ridge), Elastic Net compared across C values
- **Sparsity Analysis** — Coefficient patterns visualised per regulariser
- **Imbalance Handling** — SMOTE, random undersampling, class weighting, no correction compared
- **Comparative Analysis** — Full 3×3×2 grid: datasets × regularisers × imbalance strategies

---

## Key Findings

| Finding | Evidence |
|---------|----------|
| Preprocessing order matters | Scale-first outperforms select-first by ~2–5% PR-AUC |
| Elastic Net generalises best | Highest mean PR-AUC across all 3 datasets |
| L1 provides sparsity | 60–80% zero coefficients vs 0% for L2 |
| Class weighting is fast & effective | Competitive with SMOTE at no data cost |
| SMOTE benefits sparse models | L1 + SMOTE > L1 + class weight on extreme imbalance |

---

## Generated Figures

| Figure | Description |
|--------|-------------|
| `fig1_class_distributions.png` | Class balance across all 3 datasets |
| `fig2_pr_curves_baseline.png` | Precision-Recall curves for both pipelines |
| `fig3_overfit_underfit.png` | Train vs test F1 across 3 model configurations |
| `fig4_learning_curves.png` | Learning curves diagnosing bias-variance trade-off |
| `fig5_regularisation_comparison.png` | PR-AUC vs C for L1 / L2 / Elastic Net |
| `fig6_sparsity.png` | Coefficient sparsity patterns per regulariser |
| `fig7_imbalance_handling.png` | Precision-Recall tradeoff by imbalance strategy |
| `fig8_preprocessing_order.png` | Effect of scale-first vs select-first ordering |
| `fig9_full_heatmap.png` | PR-AUC heatmap across all conditions |

---

## Requirements

```
scikit-learn
numpy
pandas
matplotlib
seaborn
imbalanced-learn   # optional — SMOTE falls back to manual implementation if absent
```

Install with:

```bash
pip install scikit-learn numpy pandas matplotlib seaborn imbalanced-learn
```

---

## Running the Notebook

The notebook is self-contained. Run all cells top-to-bottom:

1. **Locally** — open `Seizure_Prediction_Colab.ipynb` in Jupyter or VS Code
2. **Google Colab** — upload the `.ipynb` file and run all cells

All random states are fixed at `SEED = 42`. No external data downloads are needed — datasets are generated deterministically at runtime.

---

## Repository Contents

```
├── Seizure_Prediction_Colab.ipynb        # Main notebook
├── Seizure_Prediction_Report_IEEE.docx.pdf
├── Seizure_Prediction_Presentation.pptx
├── fig1_class_distributions.png
├── fig2_pr_curves_baseline.png
├── fig3_overfit_underfit.png
├── fig4_learning_curves.png
├── fig5_regularisation_comparison.png
├── fig6_sparsity.png
├── fig7_imbalance_handling.png
├── fig8_preprocessing_order.png
└── fig9_full_heatmap.png
```
