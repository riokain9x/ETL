# ETL & Logistic Regression
## Introduction

A company which is active in Big Data and Data Science wants to hire data scientists among people who successfully pass some courses which conduct by the company.

Many people signup for their training. Company wants to know which of these candidates are really wants to work for the company after training or looking for a new employment because it helps to reduce the cost and time as well as the quality of training or planning the courses and categorization of candidates.

Information related to demographics, education, experience are in hands from candidates signup and enrollment.


## Data Sources

### 1. Enrollies' Data

As enrollies are submitting their request to join the course via Google Forms, we have the Google Sheet that stores data about enrolled students, containing the following columns:

- **enrollee_id**: Unique ID of an enrollee  
- **full_name**: Full name of an enrollee  
- **city**: The name of an enrollee's city  
- **gender**: Gender of an enrollee  

🔗 **Source**: [Google Sheet](https://docs.google.com/spreadsheets/d/1VCkHwBjJGRJ21asd9pxW4_0z2PWuKhbLR3gUHm-p4GI/edit?usp=sharing)

---

### 2. Enrollies' Education

After enrollment, everyone fills a form about their education level. This is digitalized manually and stored in Excel format.

🔗 **Source**: [Education Excel File](https://assets.swisscoding.edu.vn/company_course/enrollies_education.xlsx)

**Columns**:

- **enrollee_id**: Unique ID  
- **enrolled_university**: Enrollment status (e.g., no_enrollment, Part time course, Full time course)  
- **education_level**: Highest level of education (e.g., Graduate, Masters, etc.)  
- **major_discipline**: Major field of study (e.g., STEM, Business Degree, etc.)  

---

### 3. Enrollies' Working Experience

A survey about working experience collected manually and stored in CSV format.

🔗 **Source**: [Work Experience CSV](https://assets.swisscoding.edu.vn/company_course/work_experience.csv)

**Columns**:

- **enrollee_id**: Unique ID  
- **relevent_experience**: Has relevant experience or not  
- **experience**: Number of years of work experience (e.g., >20, <1)  
- **company_size**: Size of the company (e.g., 50−99, 100−500, etc.)  
- **company_type**: Type of company (e.g., Pvt Ltd, Funded Startup, etc.)  
- **last_new_job**: Years since last job change (e.g., never, >4, 1, etc.)  

---

### 4. Training Hours

From LMS system's database, we retrieve number of training hours completed per student.

**Database Credentials**:

- **Type**: MySQL  
- **Host**: 112.213.86.31  
- **Port**: 3360  
- **Login**: etl_practice  
- **Password**: 550814  
- **Database**: company_course  
- **Table**: training_hours  

---

### 5. City Development Index

The City Development Index (CDI) is a measure that captures the development level of cities, possibly impacting employment motivation.

🔗 **Source**: [City Development Index](https://sca-programming-school.github.io/city_development_index/index.html)

---

### 6. Employment

Employment status retrieved from the LMS database. If student is marked as employed, it means they joined the company after finishing the course.

**Database Credentials**:

- **Type**: MySQL  
- **Host**: 112.213.86.31  
- **Port**: 3360  
- **Login**: etl_practice  
- **Password**: 550814  
- **Database**: company_course  
- **Table**: employment

---

## Transform

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
