# Insurance Fraud Detection System

This repository presents an **interpretable and explainable insurance fraud detection system** developed as a portfolio project. The system leverages machine learning algorithms to identify fraudulent claims, providing actionable insights for insurance operations.

## 🎯 Business Problem

Insurance fraud poses a significant financial burden, costing the industry substantial amounts annually. The primary goal of this project is to build a robust system that not only predicts fraud but also offers transparency and interpretability, which is crucial for regulatory compliance and investigator trust. The system aims to:

- **Predict** if a claim is fraudulent (binary classification).
- Assign a **fraud probability score (0–100%)** to each claim.
- Categorize claims into **three actionable risk tiers** (High / Medium / Low) with corresponding recommended actions.
- Utilize **simple, auditable algorithms** to ensure explainability to regulators, policyholders, and internal teams.

## 💡 Key Features & Methodology

1.  **Data Loading & Preprocessing:**
    -   Loaded `Insurance_Claims.csv` dataset (1,000 claims, 39 features).
    -   Transformed `fraud_reported` into a binary (1/0) target variable.
    -   Removed irrelevant `_c39` column.
    -   **Feature Engineering:** Extracted `policy_tenure_days`, `incident_month`, and `incident_dayofweek` from date fields to capture meaningful business signals.
    -   Handled missing values using median imputation for numerical features and mode imputation for categorical features.
    -   Applied conservative outlier capping (IQR 3x multiplier) on `total_claim_amount`.

2.  **Exploratory Data Analysis (EDA):**
    -   Analyzed fraud distribution, revealing a 75/25 genuine/fraud imbalance.
    -   Investigated fraud rates by `incident_type` and `incident_severity`, noting a high fraud rate for 'Major Damage' incidents.
    -   Examined fraud patterns by `incident_hour_of_the_day`, identifying higher fraud activity during off-peak hours.
    -   Compared `total_claim_amount` between genuine and fraudulent claims.
    -   Visualized fraud rates across `policy_state`.
    -   Generated a correlation heatmap to understand feature relationships.

3.  **Feature Engineering for Modeling:**
    -   Dropped identifier columns (`policy_number`, `incident_location`) to prevent data leakage and overfitting.
    -   Applied **One-Hot Encoding** using `pd.get_dummies()` to nominal categorical variables, avoiding false ordinal rankings.

4.  **Model Training & Evaluation:**
    -   Split data into training and testing sets (80/20) with `stratify=y` to maintain fraud ratio.
    -   Scaled numerical features using `StandardScaler` for Logistic Regression.
    -   Trained and evaluated three interpretable models with `class_weight='balanced'` to address class imbalance:
        -   **Logistic Regression:** For baseline performance and coefficient-based explanations.
        -   **Decision Tree Classifier:** To generate human-readable IF-THEN rules.
        -   **Random Forest Classifier:** For robust fraud probability scoring (best AUC).
    -   **Threshold Tuning:** Optimized Random Forest for fraud recall (false negatives are costly) by sweeping probabilities, setting a production threshold of 0.30.

5.  **Interpretability & Explainability:**
    -   **Feature Importance:** Identified top fraud drivers using Random Forest's feature importances.
    -   **Logistic Regression Coefficients:** Visualized signed coefficients to explain positive/negative influences on fraud probability, crucial for regulatory compliance.
    -   **Decision Tree Visualization:** Presented top 3 levels of the Decision Tree as clear, auditable rules for investigators.
    -   **Decision Tree Text Rules:** Exported rules in plain text for manual reference.

6.  **Fraud Scoring System:**
    -   Implemented a `score_claim` function to classify claims into **HIGH, MEDIUM, LOW risk tiers** with specific recommended actions for claims handlers.
    -   Developed a **Portfolio KPI Dashboard** summarizing claim distribution across risk tiers and visualizing fraud probability scores.

7.  **SQL Integration:**
    -   Loaded raw data into an in-memory SQLite database to demonstrate how business users can run analytical SQL queries for fraud intelligence.

## 📈 Business Impact

-   The system provides a **concrete, actionable framework** for claims investigation teams.
-   A conservative estimate shows a **potential value protected** of **₹69.9 Crore annually** by effectively identifying fraudulent claims.
-   It balances fraud detection (high recall) with operational efficiency (minimizing false positives) by routing only a controlled percentage of claims for manual review.

## 📊 Model Summary

| Model                  | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|------------------------|----------|-----------|--------|----------|---------|
| Decision Tree          | 0.765    | 0.5132    | 0.7959 | 0.6240   | 0.8019  |
| Logistic Regression    | 0.775    | 0.5357    | 0.6122 | 0.5714   | 0.7812  |
| Random Forest (t=0.30) | 0.520    | 0.3309    | 0.9388 | 0.4894   | 0.8081  |

-   **Winner by F1 Score:** Decision Tree (for balanced performance on the fraud class).
-   **Winner by ROC-AUC:** Random Forest (for best ranking of suspicious claims).
-   **Winner by Recall:** Random Forest (t=0.30) (for maximizing fraud detection).
-   **Best Interpretable Model:** Decision Tree (for auditable rules).

## 🛠️ Tech Stack

-   **Python**
-   **Pandas**
-   **NumPy**
-   **Scikit-learn**
-   **Matplotlib**
-   **Seaborn**
-   **SQLite3**
