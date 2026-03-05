# Fraud Detection Pipeline

## Overview
This project implements a production-grade credit card fraud detection system that combines data-driven model selection with business-aligned operational constraints. The pipeline uses XGBoost classification with threshold tuning to balance fraud capture against false-positive rates, ensuring both security compliance and investigator efficiency.

## Approach
The analysis applies **business logic validation** throughout the workflow:
- **Data Integrity:** Validated that zero-balance values represent structural system design (merchant accounts) rather than missing data
- **Feature Engineering:** Designed features based on fraud domain knowledge (amount risk, balance volatility, temporal patterns, recipient type)
- **Model Tuning:** Applied class-weight balancing and hyperparameter optimization to handle severe class imbalance (0.13% fraud rate)
- **Threshold Optimization:** Tuned decision boundary to enforce ≥89.5% fraud recall while maximizing precision, yielding a 3.3× reduction in false positives

## Key Results
- **Model:** XGBoost (decision threshold: 0.9131)
- **Fraud Recall:** 89.5% (1,471 of 1,643 frauds detected)
- **Precision:** 30.4% (1 in 3.3 alerts is true fraud)
- **Operational Impact:** 4,832 alerts flagged across 1.27M test transactions, reducing investigator workload by 67%

## Dataset
Data sourced from [Kaggle: Fraud Detection Dataset](https://www.kaggle.com/datasets/amanalisiddiqui/fraud-detection-dataset?resource=download)

- **Size:** 6.36M transactions across training and test splits
- **Class Distribution:** 0.13% fraud cases (severe imbalance)
- **Features:** Transaction amount, balance changes, timing, recipient type, merchant flags

## Files
- `Analysis.ipynb` - Complete end-to-end analysis with business logic annotations
- `best_fraud_model_tuned.pkl` - Trained XGBoost model with optimized threshold
- `data.csv` - Transaction dataset
- `AIML Dataset.csv` - Alternative dataset format
- `Data Dictionary.txt` - Feature descriptions

## Usage
The trained model can be loaded and deployed for real-time fraud scoring:
```python
import joblib
artifact = joblib.load("best_fraud_model_tuned.pkl")
model = artifact["model"]
threshold = artifact["threshold"]
predictions = (model.predict_proba(X)[:, 1] >= threshold).astype(int)
```

## Business Impact
1. **Security:** ~90% fraud capture with minimal customer friction
2. **Efficiency:** Reduces false alarms, lowering investigator burnout and operational costs
3. **Compliance:** Maintains regulatory recall requirements while optimizing resource allocation
4. **Scalability:** Threshold-based logic enables real-time transaction processing
