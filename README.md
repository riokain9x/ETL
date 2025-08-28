# ETL & Logistic Regression
## 1. Introduction

A company which is active in Big Data and Data Science wants to hire data scientists among people who successfully pass some courses which conduct by the company.

Many people signup for their training. Company wants to know which of these candidates are really wants to work for the company after training or looking for a new employment because it helps to reduce the cost and time as well as the quality of training or planning the courses and categorization of candidates.

Information related to demographics, education, experience are in hands from candidates signup and enrollment.

## 2. Extract

We extract data from the following sources:

1. **Enrollies' Basic Information** (Google Sheet)  
    → Contains enrollee_id, name, gender, city

2. **Education Information** (Excel file)  
    → Contains enrollee_id, education_level, enrolled_university, major_discipline

3. **Working Experience** (CSV file)  
    → Contains enrollee_id, relevant_experience, experience, company_size, company_type, last_new_job

4. **Training Hours** (MySQL database - `training_hours` table)  
    → Contains enrollee_id, training_hours

5. **City Development Index** (HTML table)  
    → Contains city and city_development_index

6. **Employment Status** (MySQL database - `employment` table)  
    → Contains enrollee_id, is_employed

---

## 3. Transform

After extraction, we applied the following transformation steps:

- **Merge all sources** using `enrollee_id` or `city` as key
- **Handle missing values**:
    - Categorical: fill with "unknown"
    - Numerical: fill with median or inferred values
- **Label encoding** for all categorical columns (gender, education_level, etc.)
- **Experience column normalization**: convert values like ">20" or "<1" to numerical equivalents (e.g., 21, 0.5)
- **Standardize city names** before joining with city index

> ✔️ At the end of this phase, we have a clean dataframe with all features ready for modeling.

---

## 4. Load

Transformed data was kept in memory for modeling. Optionally, we can export the clean dataset as CSV or load to another database.

```python
df_final.to_csv("clean_enrollee_data.csv", index=False)
```

---

## 5. Model: Logistic Regression

- We split the dataset into training and testing sets (80/20)
- StandardScaler was applied to numerical features
- Logistic Regression was chosen for its interpretability and performance
- Model trained using `sklearn.linear_model.LogisticRegression`

---

## 6. Evaluation

- **Accuracy**: ~81%
- **Confusion Matrix** showed good balance between classes
- **Important features** based on coefficients:
    - `training_hours`
    - `relevent_experience`
    - `last_new_job`
    - `education_level`
    - `company_type`

```python
from sklearn.metrics import classification_report, confusion_matrix
```

---

## 7. Conclusion

- An ETL pipeline was created to integrate six data sources (Google Sheets, Excel, CSV, MySQL, HTML)
- Logistic Regression model achieved good performance in predicting employment status
- Most influential features: training hours and work experience
- This solution can help prioritize learners with high employment potential, improving recruitment planning

---

**Author**: Lucas Chen (Trần Trương Toàn Tâm)  
**Date**: August 2025
