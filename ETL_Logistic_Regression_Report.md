# 🧠 ETL & Logistic Regression Project

## 1. 📘 Introduction

A data science training company wants to identify which learners are most likely to join the company after completing their training program. Understanding this helps in optimizing training efforts and improving recruitment planning. The project involves building an ETL pipeline from various data sources and training a Logistic Regression model to predict employment outcome.

## 2. 🧾 Data Sources

| Source | Description |
|--------|-------------|
| Google Sheets | Enrollee basic info (`enrollee_id`, `city`, `gender`, etc.) |
| Excel File | Education background (`education_level`, `major_discipline`, etc.) |
| CSV File | Work experience (`experience`, `company_size`, `relevent_experience`, etc.) |
| MySQL (Table: training_hours) | Number of hours trained from LMS |
| HTML Table | City Development Index by city |
| MySQL (Table: employment) | Label indicating if student joined the company |

## 3. 🔄 ETL Pipeline

### 🔍 Extract
Used Pandas, SQLAlchemy, `gspread`, and `read_html` to fetch data from:
- Google Sheets
- Excel (Education)
- CSV (Experience)
- MySQL (Training Hours, Employment)
- Web Page (City Development Index)

### 🧹 Transform
- Standardized column names and data types
- Merged datasets on `enrollee_id`
- Encoded categorical features using LabelEncoder
- Converted experience ranges like `>20`, `<1` into numeric values
- Handled missing values with fillna or custom strategies

### 💾 Load
- Combined and cleaned dataset stored in memory for modeling
- (Optional) Save to new CSV or database for reuse

## 4. 📊 Exploratory Analysis

- Checked class imbalance for employment label
- Plotted distribution of training hours, experience, education level
- Visualized correlation between features and employment status

## 5. 🤖 Logistic Regression Modeling

### 📌 Steps
- Selected relevant features (e.g., `training_hours`, `education_level`, `experience`, `city_development_index`, etc.)
- Train/test split (80/20)
- StandardScaler applied to numeric features
- Trained Logistic Regression model from `sklearn.linear_model`

### 📈 Results
- Accuracy: ~81%
- Confusion Matrix indicates fairly balanced prediction
- Significant predictors: `training_hours`, `relevent_experience`, `last_new_job`, `education_level`, `company_type`

## 6. 💡 Insights

- More training hours → higher probability of employment
- Learners with relevant work experience are more likely to be hired
- Smaller companies and those who changed jobs recently tend to have higher motivation
- City development index adds marginal predictive value

## 7. 🏁 Conclusion

The ETL process successfully integrated data from 6 different formats/sources. The resulting Logistic Regression model provides valuable insight into which candidates are more likely to work for the company. These findings can support more effective candidate targeting and course optimization.

---

**Author**: Lucas Chen (Trần Trương Toàn Tâm)  
**GitHub**: [your-username]  
**Date**: August 2025