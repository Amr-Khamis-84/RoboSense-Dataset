# RoboSense Dataset Usage Guide

This document provides step-by-step instructions for reusing the RoboSense real-world dataset for industrial hazard detection, spatio-temporal sensor fusion, and machine learning experiments.

This guide is fully aligned with the published RoboSense real dataset and does not include any synthetic data generation or cross-domain augmentation.

---

## 1. Load and Explore the Real Dataset

```python
import pandas as pd

df = pd.read_csv('Real_Dataset.csv', parse_dates=['Timestamp'])
df.set_index('Timestamp', inplace=True)

print(df.head())
print(df.columns)
```

Each record typically contains:

- Timestamp  
- Suite_ID (AMR1, AMR2, Suite1, Suite2)  
- Sensor readings (12 modalities)  
- Optional position fields (x, y)  
- Condition label (Normal, Hazard_1, Hazard_2, Hazard_3)  

---

## 2. Filter Data by Condition or Sensor Suite

```python
normal_df  = df[df['Condition'] == 'Normal']
fire_df    = df[df['Condition'] == 'Hazard_1']
temp_df    = df[df['Condition'] == 'Hazard_2']
gas_df     = df[df['Condition'] == 'Hazard_3']

amr1_df   = df[df['Suite_ID'] == 'AMR_1']
suite1_df = df[df['Suite_ID'] == 'Suite_1']
```

---

## 3. Prepare Data for Machine Learning

```python
from sklearn.model_selection import train_test_split

X = df.drop(columns=['Condition'])

y = df['Condition'].map({
    'Normal': 0,
    'Hazard_1': 1,
    'Hazard_2': 2,
    'Hazard_3': 3
})

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42, stratify=y
)
```

---

## 4. Train a Classifier

```python
from sklearn.ensemble import RandomForestClassifier

clf = RandomForestClassifier(n_estimators=200, random_state=42)
clf.fit(X_train, y_train)
```

---

## 5. Evaluate Model Performance

```python
from sklearn.metrics import classification_report, confusion_matrix

y_pred = clf.predict(X_test)

print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
```

---

## File Description

Real_Dataset.csv: Real industrial sensor dataset containing normal operation and 15 labeled hazard events across three hazard classes.

---

## Requirements

- Python 3.7 or higher  
- pandas  
- scikit-learn  
- numpy  
- matplotlib  

Install using:

pip install pandas scikit-learn numpy matplotlib

---

## Contact

Amr Khamis  
Arab Academy for Science, Technology and Maritime Transport  
Email: amr.khamis@aast.edu  
GitHub: https://github.com/Amr-Khamis-84
