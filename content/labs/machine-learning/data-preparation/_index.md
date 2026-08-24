---
title: "Data Preparation"
description: "Preparing machine learning datasets through data cleaning, missing-value handling, encoding, scaling, splitting, and leakage prevention."
weight: 20
toc: true
---

> **Good models need good data.**
>
> Data preparation transforms raw, inconsistent data into a reliable dataset that machine learning algorithms can learn from.

---

## Why Data Preparation Matters

Real-world datasets are rarely ready for direct model training.

They may contain:

- Missing values
- Duplicate records
- Incorrect data types
- Inconsistent categories
- Outliers
- Different numerical scales
- Irrelevant columns
- Text stored as numbers
- Numerical values stored as text
- Data leakage

A typical preparation pipeline looks like:

```text
RAW DATA
   ↓
INSPECT
   ↓
CLEAN
   ↓
HANDLE MISSING VALUES
   ↓
HANDLE CATEGORICAL DATA
   ↓
SCALE / TRANSFORM
   ↓
FEATURE SELECTION
   ↓
TRAIN / TEST SPLIT
   ↓
READY FOR MODELING
```

---

## Step 1: Understand the Dataset

Before changing anything, inspect the dataset.

With Pandas:

```python
import pandas as pd

df = pd.read_csv("dataset.csv")
```

Check the first rows:

```python
print(df.head())
```

Check the shape:

```python
print(df.shape)
```

Check column names:

```python
print(df.columns)
```

Check data types:

```python
print(df.dtypes)
```

Get a summary:

```python
print(df.info())
```

Basic statistics:

```python
print(df.describe())
```

A useful first inspection is:

```text
Rows
Columns
Data Types
Missing Values
Unique Values
Numerical Statistics
Categorical Values
```

---

## Step 2: Check Missing Values

Missing values are common in real-world datasets.

Check them using:

```python
print(df.isnull().sum())
```

You can also calculate the percentage:

```python
missing_percentage = (
    df.isnull().mean() * 100
)

print(missing_percentage)
```

Example:

| Column | Missing |
|---|---:|
| Age | 2% |
| Income | 0% |
| City | 5% |
| Salary | 1% |

---

## Strategies for Missing Values

There is no single solution for every dataset.

### Option 1: Remove Rows

If only a very small number of rows contain missing values:

```python
df = df.dropna()
```

However, removing too many rows can reduce the amount of training data.

---

### Option 2: Fill Numerical Values

A common strategy is median imputation.

```python
df["Age"] = df["Age"].fillna(
    df["Age"].median()
)
```

Mean can also be used:

```python
df["Age"] = df["Age"].fillna(
    df["Age"].mean()
)
```

Median is often more robust when the data contains extreme values.

---

### Option 3: Fill Categorical Values

For categorical columns, the most frequent value can be used:

```python
df["City"] = df["City"].fillna(
    df["City"].mode()[0]
)
```

Another approach is to create an explicit category such as:

```text
Unknown
```

---

## Step 3: Remove Duplicate Records

Duplicate observations can distort model training.

Check duplicates:

```python
print(df.duplicated().sum())
```

Remove them:

```python
df = df.drop_duplicates()
```

After cleaning:

```python
print(df.shape)
```

---

## Step 4: Correct Data Types

Machine learning algorithms expect data in appropriate formats.

Check types:

```python
print(df.dtypes)
```

Convert a column:

```python
df["Age"] = df["Age"].astype(int)
```

Convert a date:

```python
df["Date"] = pd.to_datetime(df["Date"])
```

Convert text-based numerical data:

```python
df["Salary"] = pd.to_numeric(
    df["Salary"],
    errors="coerce"
)
```

Incorrect data types can create unexpected model behavior.

---

# Step 5: Handle Categorical Data

Machine learning algorithms generally require numerical input.

Suppose we have:

```text
City
-----
Pune
Mumbai
Delhi
Pune
```

We need to transform the categories.

There are several approaches.

---

## Label Encoding

Categories are mapped to numerical values.

```text
Pune   → 0
Mumbai → 1
Delhi  → 2
```

For example:

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

df["City"] = encoder.fit_transform(
    df["City"]
)
```

Label encoding is useful for some target variables, but using arbitrary numerical values for nominal input categories can introduce an artificial ordering.

---

## One-Hot Encoding

One-hot encoding creates a separate column for each category.

Original:

```text
City
-----
Pune
Mumbai
Delhi
```

Becomes:

```text
City_Pune
City_Mumbai
City_Delhi
```

Example:

```python
df = pd.get_dummies(
    df,
    columns=["City"]
)
```

This is commonly used for nominal categorical features.

---

## Using Scikit-learn

For more structured ML pipelines:

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(
    handle_unknown="ignore"
)
```

The `handle_unknown="ignore"` option helps when the test or production dataset contains a category that was not present during training.

---

# Step 6: Numerical Feature Scaling

Some algorithms are sensitive to the scale of numerical features.

Suppose:

```text
Age        = 25
Income     = 75000
Experience = 4
```

Income has a much larger numerical range.

Scaling can bring features into comparable ranges.

---

## Standardization

Standardization transforms values approximately to:

```text
mean = 0
standard deviation = 1
```

Using Scikit-learn:

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_scaled = scaler.fit_transform(X)
```

The underlying transformation is:

```text
z = (x - mean) / standard_deviation
```

Standardization is commonly useful for:

- Logistic Regression
- Linear Regression
- SVM
- KNN
- Neural networks

---

## Min-Max Scaling

Min-Max scaling transforms values into a defined range, commonly:

```text
0 to 1
```

Example:

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

X_scaled = scaler.fit_transform(X)
```

The transformation is:

```text
x_scaled = (x - min) / (max - min)
```

---

## When Scaling May Not Be Necessary

Tree-based algorithms generally do not require feature scaling in the same way distance- or gradient-sensitive methods do.

Examples:

- Decision Trees
- Random Forest
- Many Gradient Boosting implementations

The correct preprocessing depends on the algorithm.

---

# Step 7: Detect Outliers

An outlier is an observation that is unusually far from the typical values.

Example:

```text
Salary:
30000
35000
40000
42000
45000
500000
```

The value `500000` may be an outlier.

Outliers can be investigated using:

- Box plots
- Histograms
- IQR
- Z-scores
- Domain knowledge

---

## IQR Method

The Interquartile Range is:

```text
IQR = Q3 - Q1
```

Common boundaries are:

```text
Lower Bound = Q1 - 1.5 × IQR

Upper Bound = Q3 + 1.5 × IQR
```

Example:

```python
Q1 = df["Salary"].quantile(0.25)
Q3 = df["Salary"].quantile(0.75)

IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR
```

Filter observations:

```python
df_clean = df[
    (df["Salary"] >= lower) &
    (df["Salary"] <= upper)
]
```

Do not automatically remove every outlier. Some outliers are legitimate and may represent important events.

---

# Step 8: Feature Selection

Not every column should necessarily be used by the model.

Potentially unnecessary features include:

- IDs
- Duplicate information
- Constant columns
- Irrelevant metadata
- Leakage variables

For example:

```text
Customer_ID
Name
Age
Income
Purchase_History
```

`Customer_ID` may identify a record but provide no useful predictive information.

Remove unnecessary columns:

```python
df = df.drop(
    columns=["Customer_ID"]
)
```

Feature selection should be based on:

- Domain knowledge
- Correlation
- Statistical analysis
- Model-based importance
- Validation performance

---

# Step 9: Separate Features and Target

Suppose:

```text
Age
Income
Experience
Salary
```

and the goal is to predict `Salary`.

Then:

```python
X = df[
    ["Age", "Income", "Experience"]
]

y = df["Salary"]
```

Here:

```text
X = Input Features

y = Target
```

---

# Step 10: Train-Test Split

After preparing the feature and target data, split the dataset.

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

This creates:

```text
80% → Training
20% → Testing
```

For classification, stratification is often useful:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

This helps preserve class proportions between the training and test sets.

---

# Data Leakage

One of the most important concepts in data preparation is **data leakage**.

Data leakage occurs when information that should not be available during training influences the model.

This can produce unrealistically good evaluation results.

Example:

```text
Entire Dataset
      ↓
Scale Entire Dataset
      ↓
Train/Test Split
```

This is potentially problematic because information from the test set influenced the scaling parameters.

A safer approach is:

```text
Dataset
   ↓
Train/Test Split
   ↓
Fit preprocessing on Training Data
   ↓
Transform Training Data
   ↓
Transform Test Data
```

---

# The Correct Preprocessing Principle

For most learned preprocessing steps:

```text
TRAINING DATA
     ↓
fit()
     ↓
Preprocessor
     ↓
transform()
     ↓
Training Data
```

Then:

```text
TEST DATA
     ↓
transform()
     ↓
Same Preprocessor
     ↓
Test Data
```

Do not fit the preprocessing object independently on the test set.

---

# Scikit-learn Pipeline

A `Pipeline` helps keep preprocessing and modeling together.

Example:

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipeline = Pipeline([
    ("scaler", StandardScaler()),
    ("model", LogisticRegression())
])
```

Train:

```python
pipeline.fit(
    X_train,
    y_train
)
```

Predict:

```python
predictions = pipeline.predict(
    X_test
)
```

The pipeline ensures that the same preprocessing logic is applied consistently.

---

# ColumnTransformer

Real datasets often contain both numerical and categorical features.

For example:

```text
Age       → Numerical
Income    → Numerical
City      → Categorical
Education → Categorical
```

A `ColumnTransformer` can apply different preprocessing to different columns.

```python
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import (
    StandardScaler,
    OneHotEncoder
)

numeric_features = [
    "Age",
    "Income"
]

categorical_features = [
    "City",
    "Education"
]

preprocessor = ColumnTransformer([
    (
        "numeric",
        StandardScaler(),
        numeric_features
    ),
    (
        "categorical",
        OneHotEncoder(
            handle_unknown="ignore"
        ),
        categorical_features
    )
])
```

This can then be combined with a model:

```python
from sklearn.pipeline import Pipeline
from sklearn.linear_model import LogisticRegression

pipeline = Pipeline([
    ("preprocessor", preprocessor),
    ("model", LogisticRegression())
])
```

This approach is useful for production-ready ML workflows.

---

# Complete Data Preparation Pipeline

A practical pipeline can look like:

```text
RAW DATA
   ↓
Load Dataset
   ↓
Inspect
   ↓
Clean
   ↓
Remove Duplicates
   ↓
Handle Missing Values
   ↓
Correct Data Types
   ↓
Encode Categories
   ↓
Handle Outliers
   ↓
Select Features
   ↓
Split Dataset
   ↓
Fit Preprocessors on Training Data
   ↓
Transform Data
   ↓
Ready for Modeling
```

---

# Example End-to-End Preparation

```python
import pandas as pd

from sklearn.model_selection import train_test_split
from sklearn.compose import ColumnTransformer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import (
    StandardScaler,
    OneHotEncoder
)
from sklearn.linear_model import LogisticRegression
```

Load data:

```python
df = pd.read_csv("customers.csv")
```

Separate target:

```python
X = df.drop(columns=["Purchased"])
y = df["Purchased"]
```

Identify columns:

```python
numeric_features = [
    "Age",
    "Income"
]

categorical_features = [
    "City",
    "Education"
]
```

Create preprocessing:

```python
preprocessor = ColumnTransformer([
    (
        "numeric",
        StandardScaler(),
        numeric_features
    ),
    (
        "categorical",
        OneHotEncoder(
            handle_unknown="ignore"
        ),
        categorical_features
    )
])
```

Create the complete pipeline:

```python
model = Pipeline([
    ("preprocessor", preprocessor),
    ("classifier", LogisticRegression())
])
```

Split data:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

Train:

```python
model.fit(
    X_train,
    y_train
)
```

Predict:

```python
predictions = model.predict(X_test)
```

The result is a reproducible preprocessing and modeling pipeline.

---

# Common Data Preparation Mistakes

### 1. Scaling before splitting

Potential leakage can occur when scaling parameters are calculated using the entire dataset.

### 2. Fitting the encoder on test data

The encoder should be fitted using training data.

### 3. Removing too many rows

Aggressive missing-value removal can discard valuable information.

### 4. Blindly removing outliers

Some unusual observations are legitimate.

### 5. Encoding categories incorrectly

Nominal categories should not automatically be treated as ordered numerical values.

### 6. Keeping irrelevant identifiers

IDs may add noise without providing useful predictive information.

### 7. Ignoring data types

Numbers stored as strings can cause incorrect processing.

### 8. Using test data during feature engineering

Information from the test set should not influence training decisions.

---

# Interview Questions

### Why is data preprocessing important?

Because real-world data is often incomplete, inconsistent, noisy, or represented in formats that machine learning algorithms cannot directly use.

### What is data leakage?

Data leakage occurs when information unavailable at prediction time influences model training or evaluation.

### Why do we scale features?

Scaling puts numerical features on comparable ranges and can improve algorithms that are sensitive to feature magnitude.

### What is the difference between StandardScaler and MinMaxScaler?

`StandardScaler` standardizes values around mean 0 with standard deviation 1, while `MinMaxScaler` maps values into a specified range, commonly 0 to 1.

### When should we use one-hot encoding?

One-hot encoding is commonly used for nominal categorical variables where categories do not have a natural numerical order.

### Why use a Pipeline?

A Pipeline keeps preprocessing and modeling steps together and helps ensure consistent transformations during training and prediction.

### What is the purpose of ColumnTransformer?

It allows different preprocessing operations to be applied to different groups of columns.

---

# Key Takeaways

```text
DATA PREPARATION
       │
       ├── Inspect
       ├── Clean
       ├── Handle Missing Values
       ├── Remove Duplicates
       ├── Correct Data Types
       ├── Encode Categories
       ├── Scale Features
       ├── Investigate Outliers
       ├── Select Features
       ├── Split Data
       └── Prevent Leakage
```

The most important rule is:

> **Never allow information from the test set to influence the training process.**

A well-designed preprocessing pipeline improves reproducibility, prevents leakage, and makes the model easier to deploy.

---

## Lab Takeaway

**Data preparation is part of machine learning, not a separate cleanup step.**

The quality of the final model depends heavily on how carefully the raw data is transformed into reliable features.

```text
RAW DATA
   ↓
CLEAN
   ↓
TRANSFORM
   ↓
SPLIT
   ↓
PREPROCESS
   ↓
MODEL
```

The next step is **Exploratory Data Analysis**, where we investigate the structure, distributions, relationships, and patterns hidden inside the prepared dataset.
