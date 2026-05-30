#  Customer Sales Data Cleaning Using Python

##  Project Overview

This project focuses on cleaning a messy customer sales dataset using Python, Pandas, NumPy, and Regular Expressions (Regex).

The dataset contains:
- Missing Values
- Duplicate Records
- Inconsistent Formatting
- Incorrect Data Types
- Dirty Text Data

The goal is to prepare the dataset for Data Analysis and Machine Learning.

---

#  Tools Used

- Python
- Pandas
- NumPy
- Regex
- Google Colab

---

#  Import Libraries

```python
import pandas as pd
import numpy as np
import re
```

# Load Dataset

```python
df = pd.read_csv("messy_customer_sales_data.csv")
```

---

#  Data Exploration

## Dataset Information

```python
df.info()
```

## Statistical Summary

```python
df.describe().round()
```

## Missing Values

```python
df.isnull().sum()
```

## Dataset Shape

```python
df.shape
```

## Duplicate Records

```python
df[df.duplicated()]
```

---

#  Handling Missing Values

## Check Missing Values

```python
df.isnull().sum()
```

### Extract Age from Dirty Values

Example:

```text
25 years
30 years
nan years
```

```python
def extract_age(age):
    age_num = re.findall('[0-9]+', str(age))

    if len(age_num) > 0:
        return age_num[0]
    else:
        return age

df['Age'] = df['Age'].apply(
    lambda x: extract_age(x)
)
```

### Calculate Median Age

```python
df_age = df[df['Age'] != 'nan years']['Age']

age_median = int(
    df_age.dropna()
          .astype('int64')
          .median()
)
```

### Fill Missing Age Values

```python
df.replace(
    'nan years',
    age_median,
    inplace=True
)

df['Age'].fillna(
    age_median,
    inplace=True
)
```

### Fill Purchase Amount

```python
df['Purchase_Amount'].fillna(
    df['Purchase_Amount'].median(),
    inplace=True
)
```

### Fill Categorical Columns

```python
for col in [
    'Gender',
    'City',
    'Country',
    'Feedback_Score',
    'Last_Purchase_Date'
]:
    df[col].fillna(
        df[col].mode()[0],
        inplace=True
    )
```

### Remove Missing Customer IDs

```python
df.dropna(
    subset='Customer_ID',
    inplace=True
)
```

---

#  Standardizing Data

## Gender Cleaning

Before:

```text
Male
male
M
Female
F
```

Code:

```python
df['Gender'] = (
    df['Gender']
    .str.lower()
    .str.strip()
)

df['Gender'].replace(
    {
        'm':'male',
        'f':'female'
    },
    inplace=True
)
```

After:

```text
male
female
```

---

## Country Cleaning

```python
df['Country'] = (
    df['Country']
    .str.lower()
)

df['Country'].replace(
    {
        'ind':'india'
    },
    inplace=True
)
```

---

#  Handling Duplicates

## Find Duplicates

```python
df[df.duplicated()]
```

## Remove Duplicate Rows

```python
df.drop_duplicates(
    inplace=True
)
```

## Remove Duplicate Customer IDs

```python
df.drop_duplicates(
    subset=['Customer_ID'],
    keep='first',
    inplace=True
)
```

---

#  Correct Data Types

## Convert Age to Integer

```python
df['Age'] = df['Age'].astype('int64')
```

## Convert Signup Date

```python
df['Signup_Date'] = pd.to_datetime(
    df['Signup_Date']
)
```

## Convert Last Purchase Date

```python
df['Last_Purchase_Date'] = pd.to_datetime(
    df['Last_Purchase_Date']
)
```

## Verify Data Types

```python
df.dtypes
```

---

# Skills Demonstrated

Data Cleaning

Missing Value Treatment

Duplicate Handling

Data Standardization

Regex

Data Type Conversion

Pandas

NumPy

Data Preprocessing

---

# Project Outcome

Successfully transformed a messy customer sales dataset into a clean and analysis-ready dataset by applying industry-standard data cleaning techniques.

The cleaned dataset can now be used for:

- Exploratory Data Analysis (EDA)
- Dashboard Creation
- Business Analytics
- Machine Learning Models

---

# Author Gaurav Kumar 

Gaurav Kumar

Data Analyst | SQL | Python | Power BI | Excel
