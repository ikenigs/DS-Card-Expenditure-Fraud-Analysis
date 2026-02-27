# DS-Card-Expenditure-Fraud-Analysis
# Toronto Government P-Card Analysis  
**Identifying Anomalies and Strengthening Spending Controls**

## 📌 Project Overview

This project analyzes Corporate Purchasing Card (P-Card) transaction data from the City of Toronto Open Data Portal to understand spending behavior and identify potential fraud or misuse patterns.

The dataset contains over **151,000 transactions** across **81 city divisions**, covering operational purchases made with government-issued corporate cards.

The objective is to:

- Understand how P-Cards are used across divisions  
- Identify behavioral spending patterns  
- Detect high-risk or unusual transactions  
- Build a multi-layered fraud detection framework combining statistical analysis and machine learning  

---

## 📊 Dataset Summary

- **151,425 transactions**
- **$50.4M total spend**
- **$330 average transaction amount**
- **$103 median transaction amount**
- 81 divisions
- 295 merchant categories
- 33,000+ raw merchant names (standardized during cleaning)

Key characteristics:

- 75% of transactions are under $300  
- Spending is stable month-to-month  
- 8% of transactions occur on weekends  
- 1.7% are foreign-currency transactions  

---

## 🔎 Analytical Approach

The analysis was structured into multiple layers:

### 1️⃣ Exploratory Data Analysis (EDA)

- Distribution analysis of transaction amounts (highly right-skewed)
- Division-level spending behavior
- Merchant category analysis
- Monthly trend evaluation
- Weekend vs weekday comparisons
- Foreign vs CAD transaction comparison

Key finding:  
The mean transaction amount is statistically different from $300 (one-sample t-test, p < 0.001), although the practical difference is modest.

---

### 2️⃣ Hypothesis Testing

Formal statistical tests were applied to evaluate fraud-related assumptions:

- Is the average transaction equal to $300?
- Is weekend spending different from weekday spending?
- Is weekend spending higher?
- Does Economic Development & Culture use foreign currency more often than other divisions?

Methods used:

- One-sample t-test  
- Two-sample t-test  
- Welch’s t-test  
- Two-proportion z-test  

---

### 3️⃣ Feature Engineering

New variables were created to capture behavioral signals:

- `is_weekend_flag`
- `is_foreign`
- `zscore_outlier`
- `business_outlier` (≥ $5,000 threshold)
- `outlier_flag`
- `iso_flag_simple` (Isolation Forest)
- `final_merchant` (standardized merchant field)
- `month_num`
- `weekday`

#### Merchant Standardization

The dataset originally contained 33,000+ unique merchant names due to inconsistent formatting.

A rule-based cleaning approach:

- Removed numbers and special characters
- Converted to uppercase
- Collapsed whitespace
- Extracted core merchant words

This reduced unique merchants by **56%**, significantly improving merchant-level analysis.

LLM normalization and fuzzy matching were tested but produced unreliable merges and were not used in the final model.

---

### 4️⃣ Outlier Detection Framework

Transactions were flagged using multiple rule-based criteria:

- Z-score outliers (|z| > 3)
- High-value purchases ≥ $5,000
- Weekend transactions
- Foreign currency transactions

Results:

- 1,602 z-score outliers  
- 1,512 high-value transactions  
- 1,629 foreign transactions  
- 3,029 transactions triggered at least one flag  

This created a layered risk-filtering system rather than a single threshold rule.

---

### 5️⃣ Machine Learning – Isolation Forest

An **Isolation Forest** model was implemented to detect anomalies using:

- Transaction amount  
- Currency  
- Day of week  
- Division behavior  

The model:

- Confirmed obvious outliers captured by business rules  
- Detected subtler anomalies that threshold rules could not identify  
- Improved detection by increasing contamination to 2%  

The ML model did not replace business logic — it strengthened it.

---

## 🧠 Key Insights

- No evidence of systematic misuse across time, divisions, or merchant categories.
- A small fraction of activity (~11% of total spend) exhibits elevated risk characteristics.
- Rule-based detection alone generates false positives.
- Combining business rules with machine learning produces a stronger and more scalable framework.
- Merchant-level behavior modeling is essential for fraud detection in procurement data.

---

## 🚀 Recommended Improvements

### Data Model Enhancements

- Include `employee_id`
- Include employee category
- Capture transaction timestamps
- Collect labeled fraud cases

### Merchant Controls

- Division-specific merchant whitelists
- Division-specific blacklists

### Multi-layered Fraud Strategy

- Combine rule-based flags
- Merchant behavior analysis
- Division spending patterns
- Machine learning anomaly detection

---

## 🛠 Tech Stack

- Python  
- Pandas  
- NumPy  
- Matplotlib / Seaborn  
- SciPy  
- Scikit-learn  
- Jupyter Notebook  

