# Credit Card Fraud Detection System

**End-to-End Fraud Analytics Pipeline with Cost-Aware Multi-Stage Decision Engine**

An intelligent, production-oriented fraud detection system built on the classic **[Credit Card Fraud Dataset](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)**. This project demonstrates how to go beyond simple classification by incorporating domain knowledge, leakage-safe engineering, and real business cost optimization.

![Fraud Detection Dashboard](newplot%20(3).png)

## 📊 Final Results (After Fixes)

| Metric              | Value      | 
|---------------------|------------|
| Total Transactions  | 56,962     |
| Total Fraud Cases   | 75         |
| **Fraud Detected**  | **64**     |
| **Fraud Missed**    | **11**     |
| **Fraud Detection Rate** | **85.33%** |
| Baseline Cost       | ₹37,500    |
| System Cost         | ₹5,980     |
| **Net Savings**     | **₹31,520** |

**Huge improvement after adding V1–V28 features and fixing leakage!**

---

## ✨ Project Highlights

- Time-based chronological split (realistic for fraud)
- Rich feature engineering focused on abnormal behavior
- Leakage-safe rolling fraud density
- Cost-sensitive threshold optimization
- Multi-stage decision system (Approve / Review / Block)
- Top-N risk-based strategy
- Comprehensive evaluation using fraud-specific metrics

---

## 🛠 Tech Stack

- **Language**: Python 3.9+
- **Data Manipulation**: Pandas, NumPy
- **Machine Learning**: Scikit-learn
- **Visualization**: Plotly, Matplotlib
- **Pipeline**: Scikit-learn `Pipeline`

---

## 📁 Project Structure

```bash
credit-card-fraud-detection/
├── data/
│   └── creditcard.csv
├── notebooks/
│   └── Fraud_Detection_Pipeline.ipynb
├── src/
│   ├── features.py
│   ├── models.py
│   └── decision_engine.py
├── visualizations/
├── README.md
└── requirements.txt



Key Concepts & Methods Used
1. Problem Understanding
Credit card fraud is a highly imbalanced classification problem (≈0.17% fraud rate). Traditional accuracy is useless. I focus on:

High Recall for fraud (minimize financial loss)
Controlled False Positive Rate (minimize customer friction)

2. Data Splitting Strategy
split_index = int(len(df) * 0.8)
train_df = df.iloc[:split_index].copy()
test_df = df.iloc[split_index:].copy()

3. Feature Engineering (Core Strength)
I followed three strict principles:

Features must represent abnormal behavior, not raw values
Must be real-time computable
Must be leakage-free

 Engineered Features  Purpose                             
 log_amount          Handle right-skewed amounts   
 amount_risk_band    Micro / Low / Medium / High risk
 hour                Transaction timing
 night_txn           Flag for low-monitoring hours (0-6)
 amount_deviation    Z-score from training mean
 rolling_fraud_rate  Recent fraud density
 rolling_fraud_rate  Anonymized behavioral patterns

4. Models Used

Baseline: Logistic Regression + Probability Calibration (Sigmoid)
Main Model: Random Forest Classifier (300 trees, balanced_subsample)
Both wrapped in StandardScaler + Pipeline.

5. Fraud Detection Evaluation Metrics Explained
Standard Metrics
ROC-AUC: Measures ranking ability (0.5 = random, 1.0 = perfect). Good for overall discrimination.
PR-AUC (Average Precision): Much more informative than ROC-AUC in highly imbalanced datasets like fraud.

 Business-Oriented Metrics

 Confusion Matrix components:
TP: Correctly caught fraud
FN (False Negative): Missed fraud → Direct financial loss (₹500 each)
FP (False Positive): Legitimate transaction blocked → Customer friction (₹10 each)
TN: Correctly approved genuine transactions

Cost Matrix:
FN_COST = 500      # Missed fraud loss
FP_COST = 10       # False block cost
REVIEW_COST = 2    # Manual review cost
Detection Rate (Recall for fraud class): % of actual frauds caught
Net Savings: Baseline_Cost - System_Cost

 6. Decision Engine (Multi-Stage System)
Instead of simple binary classification, I implemented a risk-based decision framework:

def assign_decision(prob):
    if prob >= HIGH_RISK:      # 0.20
        return "Block"
    elif prob >= MEDIUM_RISK:  # 0.05
        return "Review"
    else:
        return "Approve"


 Author
PantherPink
Data Scientist specializing in Fraud Analytics & Risk Modeling
