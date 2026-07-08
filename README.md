# 📉 Aftershoot Pro - Customer Churn Prediction

## 📌 Project Overview

Customer retention is one of the most important challenges for subscription-based businesses.

This project builds an end-to-end Machine Learning pipeline to predict customer churn for Aftershoot Pro using monthly user activity data. The objective is to identify users at risk of leaving the platform and provide actionable insights that can support retention strategies.

The project covers the complete Data Science workflow:

* Business understanding
* Churn label creation
* Exploratory Data Analysis (EDA)
* Feature Engineering
* Behavioral Trend Analysis
* Machine Learning Modeling
* Model Evaluation
* Business Recommendations

---

## 🎯 Business Problem

Aftershoot Pro users generate monthly activity logs while using the platform.

The goal is to answer:

> Which users are most likely to churn based on their historical product usage behavior?

Accurately identifying high-risk users enables:

* Proactive customer retention
* Targeted marketing campaigns
* Feature adoption programs
* Customer success interventions

---

## 📂 Dataset Description

Each row represents one active user during one month.

### Available Features

| Feature            | Description                |
| ------------------ | -------------------------- |
| user_id            | Unique user identifier     |
| active_month       | Activity month             |
| first_payment_date | Subscription start date    |
| total_projects     | Number of projects created |
| culls              | Culling usage count        |
| edits              | Editing usage count        |
| retouchs           | Retouching usage count     |

---

# 🏷️ Churn Definition

The dataset did not contain a churn label.

Therefore, churn was engineered using user activity history.

### Initial Approach

A user was considered churned if:

```python
next_month.isna()
```

This approach produced weak model performance because users in the final observed month were automatically labeled as churned.

---

### Final Churn Definition

A user is considered churned if:

* There is a gap of more than 2 months before their next activity
  OR
* There is no future activity recorded

```python
churn = (
    (month_gap > 2)
    |
    (next_month.isna())
)
```

This definition better reflects real-world subscription churn behavior.

---

# 🔍 Exploratory Data Analysis

### Average Projects

| User Type      | Average Projects |
| -------------- | ---------------- |
| Retained Users | 8.82             |
| Churned Users  | 5.70             |

### Average Usage

| User Type      | Average Usage |
| -------------- | ------------- |
| Retained Users | 10.72         |
| Churned Users  | 6.33          |

### Key Insight

Users who churn show significantly lower engagement levels compared to retained users.

---

# ⚙️ Feature Engineering

## Engagement Features

```python
total_usage
features_used
tenure_months
```

### Total Usage

Measures overall platform engagement.

```python
total_usage =
culls + edits + retouchs
```

---

### Features Used

Measures product adoption.

```python
features_used
```

Counts how many product modules the user actively used.

---

### Tenure

Measures customer lifetime.

```python
tenure_months
```

---

## Behavioral Trend Features

To capture engagement trends over time:

### Rolling Features

```python
rolling_projects
rolling_usage
```

3-month moving averages.

---

### Lag Features

```python
prev_usage
prev_projects
```

Previous month's activity.

---

### Change Features

```python
usage_change
project_change
```

Measure month-over-month engagement changes.

---

# 🤖 Machine Learning Models

Three models were trained and evaluated.

## 1. Logistic Regression

Used as a baseline interpretable model.

### Results

* ROC-AUC: 0.694

---

## 2. Random Forest

Used to capture nonlinear relationships.

### Results

* ROC-AUC: 0.620

---

## 3. XGBoost (Final Model)

Used for improved predictive performance.

### Results

| Metric    | Score |
| --------- | ----- |
| Accuracy  | 66.6% |
| Precision | 16.4% |
| Recall    | 66.1% |
| F1 Score  | 26.2% |
| ROC-AUC   | 70.5% |

---

# 📊 Feature Importance

Top churn drivers identified by XGBoost:

| Rank | Feature          |
| ---- | ---------------- |
| 1    | Edits            |
| 2    | Total Usage      |
| 3    | Previous Usage   |
| 4    | Rolling Usage    |
| 5    | Rolling Projects |
| 6    | Tenure Months    |

### Key Finding

Editing activity and overall engagement were the strongest predictors of future churn.

Users exhibiting declining usage patterns were significantly more likely to leave the platform.

---

# 🚧 Challenges Faced

This project involved several real-world challenges:

### Challenge 1

No churn label available.

### Solution

Created churn labels using activity gaps.

---

### Challenge 2

Incorrect initial churn definition.

### Solution

Redesigned churn logic using inactivity windows.

---

### Challenge 3

Dataset boundary (right-censoring) issue.

### Solution

Excluded final observation periods where churn could not be reliably observed.

---

### Challenge 4

Missing values introduced by lag features.

### Solution

Handled using feature-specific imputation.

---

### Challenge 5

Models initially predicted all users as non-churn.

### Solution

Investigated probability distributions and optimized classification thresholds.

---

# 💡 Business Recommendations

Based on the model findings:

### High-Risk Users

Users showing:

* Declining edits
* Lower project creation
* Reduced overall usage

should be proactively targeted.

### Recommended Retention Actions

* Personalized onboarding
* Feature education campaigns
* Customer success outreach
* Subscription incentives
* Usage re-engagement programs

---

# 🛠️ Tech Stack

* Python
* Pandas
* NumPy
* Matplotlib
* Scikit-Learn
* XGBoost
* Jupyter Notebook

---

# 📈 Future Improvements

Potential enhancements include:

* Hyperparameter tuning using Optuna
* SMOTE for class imbalance handling
* SHAP explainability analysis
* Survival Analysis for churn forecasting
* Ensemble modeling
* Real-time churn monitoring dashboard

---

# 👨‍💻 Author

Rhythm 

Former Data Analyst @Octro.inc | Data Science & Machine Learning Enthusiast

Open to opportunities in:

* Data Science
* Machine Learning
* Data Analytics
* Business Analytics
