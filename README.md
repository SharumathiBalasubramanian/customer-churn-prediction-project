# Interpretable AI : SHAP Analysis of Customer Churn Prediction Model
Dataset: IBM Telco Customer Churn
Model: XGBoost (Best Hyperparameters Selected via 4-Fold RandomizedSearchCV)
# Project overview
Customer churn prediction is essential for telecom operators to retain valuable customers. This project aims to build a robust machine learning model that predicts churn and, more importantly, explains why a customer is predicted to churn using interpretable AI techniques such as SHAP (SHapley Additive exPlanations).

# Dataset Description
Source: IBM Telco Customer Churn Dataset (Kaggle)
Link: https://www.kaggle.com/datasets/blastchar/telco-customer-churn
-Target Variable
Churn (Yes / No)
- Number of Rows & Columns
Rows: 7043
Columns: 21
# Main Feature Categories
**Category	Examples**
- Customer Demographics	Gender, SeniorCitizen, Partner, Dependents
- Customer Account Info	Tenure, Contract, PaymentMethod, PaperlessBilling
- Services Signed Up	PhoneService, InternetService, StreamingTV, StreamingMovies
- Financial	MonthlyCharges, TotalCharges
# Data Quality Notes
“TotalCharges” contains whitespace → converted to numeric.
# Data Preprocessing
- Handled missing values
Cleaned whitespace in TotalCharges
Converted to float & imputed missing values (median)
-  Dropped unnecessary ID column
customerID removed
- Encoding
Binary categorical → Label Encoding
Low-cardinality categorical → One-hot encoding
Resulting feature count: 33
# Model Development & Hyperparameter Tuning
| **Parameter**    | **Value**          |
| ---------------- | ------------------ |
| Model Used       | XGBoostClassifier  |
| Tuning Method    | RandomizedSearchCV |
| Iterations       | 30                 |
| Cross-Validation | 4-fold CV          |
| **Hyperparameter** | **Value** |
| ------------------ | --------- |
| subsample          | 1.0       |
| reg_lambda         | 2         |
| reg_alpha          | 1         |
| n_estimators       | 800       |
| max_depth          | 5         |
| learning_rate      | 0.05      |
| colsample_bytree   | 0.5       |
| **Aspect**              | **Meaning**                                      |
| ----------------------- | ------------------------------------------------ |
| Model Depth             | Deeper model (max_depth = 5)                     |
| Regularization Strength | Strong regularization (α = 1, λ = 2)             |
| Number of Trees         | Large number of trees (800)                      |
| Generalization          | Good generalization due to subsample & colsample |

# Model Performance
Test Performance Metrics
| **Metric**           | **Value** |
| -------------------- | --------- |
| Test AUC             | 0.8320    |
| Accuracy             | 0.7913    |
| Precision (No Churn) | 0.84      |
| Recall (No Churn)    | 0.89      |
| Precision (Churn)    | 0.63      |
| Recall (Churn)       | 0.52      |
# Interpretation:
Model is reasonably good at identifying churn (52% recall)
Very strong at identifying non-churn customers (89% recall)
AUC 0.83 indicates solid separation ability
# 6. Global SHAP Interpretation
Global SHAP analysis identifies the most influential features driving churn across the entire dataset.
**Top Features Increasing Churn (from SHAP summary):**
- Month-to-Month Contract
- High Monthly Charges
- Short Tenure
- Fiber Optic Internet
- Paperless Billing
- Lack of OnlineSecurity/TechSupport
- Electronic Payment Method (e.g., E-check)
# Global Interpretation Summary
Customers with shorter tenure are far more likely to churn.
Month-to-Month contracts strongly push SHAP values towards churn.
High MonthlyCharges consistently increase churn SHAP scores.
Customers using Fiber Optic internet have higher churn—likely due to high cost or service issues.
# Local SHAP Explanations (5 Selected Customers)
**Customers were chosen automatically based on:**
-Top 3 highest predicted churn probabilities
-Bottom 2 lowest predicted churn probabilities
-Selected Indices:
| **Customer ID** | **Risk Level**       | **Top SHAP Drivers (Reasons)**                                                                                             | **Summary**                                                                          |
| --------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **2631**        | High-Risk Churner    | • Month-to-month contract<br>• Very high monthly charges<br>• Short tenure<br>• Fiber optic service<br>• Paperless billing | High churn risk due to high cost, low loyalty signals, and possible dissatisfaction. |
| **3536**        | High-Risk Churner    | • High monthly charges<br>• No online security services<br>• Short tenure<br>• Month-to-month contract                     | High spend + lack of value-added services + weak contract commitment → churn risk.   |
| **6866**        | High-Risk Churner    | • Low tenure<br>• Fiber optic internet<br>• High bill amount<br>• Month-to-month billing                                   | Classic churner profile: new customer + expensive plan + no contract commitment.     |
| **3814**        | Low-Risk Non-Churner | • Two-year contract<br>• Long tenure<br>• Stable monthly charges<br>• Multiple services                                    | Loyal long-term customer, financially stable with strong retention signals.          |
| **4267**        | Low-Risk Non-Churner | • One- or two-year contract<br>• Moderate/low monthly charges<br>• Uses value-added services<br>• Long tenure              | Stable long-term customer with strong service utilization and low churn tendency.    |

# Business Translation (Actionable Recommendations)
- Convert Month-to-Month Users to Long-Term Plans
Offer small discounts or loyalty rewards to get them into 1- or 2-year contracts.
- Provide Bill Relief for High-Charge Customers
Introduce personalized discounts or bundle offers for customers with high monthly bills.
-Intervene Early With High-Complaint Customers
Create a priority support list for customers making multiple service calls.
- Increase Adoption of Add-On Services
Offer free trials or bundle upgrades to make customers more engaged and less likely to leave.
- Reduce Reliance on Electronic Check Payments
Provide incentives for switching to autopay or credit card billing.
