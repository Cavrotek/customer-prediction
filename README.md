# ExtraaLearn: Potential Customers Prediction

Predicting which leads of an EdTech start-up (ExtraaLearn) are likely to convert to paid customers, identifying the factors that drive conversion, and profiling the leads most likely to convert.

## Results at a glance

| Model (tuned) | Test ROC-AUC | Test recall | Test precision | Test F1 |
|---|---|---|---|---|
| **Random Forest (final)** | 0.925 | 0.84 | 0.69 | 0.76 |
| XGBoost | 0.925 | 0.84 | 0.69 | 0.76 |
| Gradient Boosting | 0.924 | 0.83 | 0.69 | 0.76 |
| Decision Tree | 0.911 | 0.85 | 0.65 | 0.74 |
| Logistic Regression | 0.876 | 0.85 | 0.60 | 0.71 |

- The final **Random Forest** flags 37% of leads, and that group contains **84% of the actual converters**. 69% of flagged leads convert, against a 30% base rate.
- **XGBoost did not add to the score.** Across 15 paired cross-validation folds the mean ROC-AUC difference from Random Forest is -0.0003, so the two are statistically indistinguishable.
- **Top drivers:** first interaction via the website (vs the mobile app), time spent on the website, and profile completion. Advertising channels show no measurable effect.
- **Likely converter profile:** a working professional aged about 45 to 60 who first came through the website, has a highly completed profile, and spends a long time on the site. This segment converts at 83%.

## Project structure

```
├── data/
│   └── ExtraaLearn.csv          # 4,612 leads, 15 columns
├── notebooks/
│   └── ExtraaLearn - Potential Customers Prediction.ipynb
├── reports/
│   ├── Report - Potential Customers Prediction.docx
│   └── Report - Potential Customers Prediction.pdf
├── Context.txt                  # problem statement and data dictionary
├── requirements.txt
└── README.md
```

## How to run

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate    macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/
```

The notebook reads `../data/ExtraaLearn.csv`. Model tuning takes about 2 to 3 minutes. All random seeds are fixed (`random_state=1`), so the results are reproducible.

## Approach

1. Data overview and exploratory analysis (univariate, bivariate, five business questions)
2. Pre-processing: one-hot encoding, stratified 70/30 train/test split, class weighting for the 70/30 imbalance
3. Decision Tree and Random Forest, default and hyper-tuned (5-fold CV, ROC-AUC), plus Logistic Regression, Gradient Boosting and XGBoost benchmarks
4. Decision thresholds set on out-of-fold training predictions to catch 85% of converters, then evaluated on the held-out test set
5. Feature importance (impurity-based and permutation), lead profile, and business recommendations
