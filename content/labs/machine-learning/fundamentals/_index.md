---
title: "Machine Learning Fundamentals"
description: "Understanding the foundations of machine learning, learning paradigms, datasets, features, models, training, validation, testing, and the complete ML workflow."
weight: 10
toc: true
---

> **Machine learning is not just about choosing an algorithm.**
>
> It is about turning data into a system that can learn patterns and make useful predictions on new data.

---

## What is Machine Learning?

Machine Learning (ML) is a branch of Artificial Intelligence where computer systems learn patterns from data and use those patterns to make predictions, classifications, or decisions.

Instead of explicitly programming every rule, we provide:

```text
DATA
  ↓
LEARNING ALGORITHM
  ↓
MODEL
  ↓
PREDICTION
```

For example, instead of manually writing rules to identify whether an email is spam, we can provide thousands of examples of spam and legitimate emails.

The machine learning algorithm learns patterns from those examples.

---

## AI → Machine Learning → Deep Learning

These concepts are related but not identical.

```text
Artificial Intelligence
│
├── Machine Learning
│   │
│   ├── Classical ML
│   │   ├── Linear Regression
│   │   ├── Logistic Regression
│   │   ├── Decision Trees
│   │   ├── Random Forest
│   │   └── SVM
│   │
│   └── Deep Learning
│       ├── CNN
│       ├── RNN
│       ├── Transformers
│       └── U-Net
│
└── Other AI approaches
```

### Artificial Intelligence

The broad field of building systems capable of performing tasks that normally require human intelligence.

### Machine Learning

A subset of AI where systems learn patterns from data.

### Deep Learning

A subset of machine learning based primarily on multi-layer neural networks.

---

## Types of Machine Learning

The three major learning paradigms are:

```text
Machine Learning
│
├── Supervised Learning
│
├── Unsupervised Learning
│
└── Reinforcement Learning
```

---

## 1. Supervised Learning

In supervised learning, the training data contains known answers.

```text
Input Features
      +
Known Target
      ↓
Training
      ↓
Model
      ↓
Prediction
```

For example:

| Age | Income | Purchased |
|---:|---:|---:|
| 25 | 30000 | No |
| 32 | 55000 | Yes |
| 45 | 80000 | Yes |
| 22 | 25000 | No |

The model learns the relationship between the input features and the target.

### Common supervised learning tasks

#### Regression

Predicting a continuous numerical value.

Examples:

- House price
- Temperature
- Sales
- Salary
- Demand

#### Classification

Predicting a category.

Examples:

- Spam / Not Spam
- Disease / Healthy
- Fraud / Legitimate
- Cat / Dog

---

## 2. Unsupervised Learning

In unsupervised learning, the dataset does not contain predefined target labels.

```text
Raw Data
   ↓
Algorithm
   ↓
Discover Patterns
   ↓
Groups / Structure
```

Common applications include:

- Customer segmentation
- Clustering
- Anomaly detection
- Dimensionality reduction
- Pattern discovery

Common algorithms:

- K-Means
- DBSCAN
- Hierarchical Clustering
- PCA

---

## 3. Reinforcement Learning

Reinforcement learning is based on an agent interacting with an environment.

```text
        ┌──────────────┐
        │ Environment  │
        └──────┬───────┘
               │
            State
               ↓
        ┌──────────────┐
        │    Agent     │
        └──────┬───────┘
               │
            Action
               ↓
        ┌──────────────┐
        │ Environment  │
        └──────┬───────┘
               │
            Reward
               ↓
             Agent
```

The agent learns which actions produce better rewards.

Applications include:

- Robotics
- Game playing
- Autonomous systems
- Resource optimization

---

## Important Machine Learning Terminology

Understanding the vocabulary is essential when working with ML models.

### Dataset

A collection of observations used for analysis and machine learning.

Example:

```text
customer_data.csv
```

may contain thousands of customer records.

### Sample

A single observation or row in a dataset.

```text
Age = 35
Income = 75000
Experience = 10
```

represents one sample.

### Feature

An input variable used by the model.

Example:

```text
Age
Income
Experience
Education
```

These are features.

### Target / Label

The value the model is trying to predict.

For example:

```text
Features:
Age
Income
Experience

Target:
Salary
```

### Model

A mathematical representation of patterns learned from training data.

```text
Training Data
     ↓
Learning Algorithm
     ↓
Trained Model
```

### Parameter

A value learned by the model during training.

For example, linear regression learns coefficients.

```text
y = w1x1 + w2x2 + b
```

The model learns:

```text
w1
w2
b
```

### Hyperparameter

A configuration value selected before or during model training.

Examples:

```text
learning_rate
max_depth
n_estimators
k
batch_size
```

Hyperparameters are not directly learned from the training data in the same way as model parameters.

---

## Training, Validation and Test Data

A common machine learning workflow divides the dataset into separate subsets.

```text
Complete Dataset
       │
       ├───────────────┐
       ↓               ↓
   Training          Test
       │
       ↓
   Validation
```

A more practical representation is:

```text
Dataset
│
├── Training Set
│      ↓
│   Learn patterns
│
├── Validation Set
│      ↓
│   Tune model
│
└── Test Set
       ↓
    Final evaluation
```

Typical splits might be:

```text
70% Training
15% Validation
15% Testing
```

or:

```text
80% Training
20% Testing
```

The exact split depends on the dataset and problem.

---

## The Machine Learning Workflow

A practical ML project usually follows this pipeline:

```text
Problem Definition
        ↓
Data Collection
        ↓
Data Cleaning
        ↓
Exploratory Data Analysis
        ↓
Feature Engineering
        ↓
Train / Validation / Test Split
        ↓
Model Selection
        ↓
Training
        ↓
Evaluation
        ↓
Hyperparameter Tuning
        ↓
Final Model
        ↓
Deployment
        ↓
Monitoring
```

The algorithm is only one part of this process.

---

## Common Machine Learning Algorithms

### Regression

Used when predicting continuous values.

Common algorithms:

- Linear Regression
- Polynomial Regression
- Ridge Regression
- Lasso Regression

### Classification

Common algorithms:

- Logistic Regression
- K-Nearest Neighbors
- Support Vector Machine
- Decision Tree
- Random Forest
- Gradient Boosting

### Clustering

Common algorithms:

- K-Means
- DBSCAN
- Hierarchical Clustering

### Dimensionality Reduction

Common techniques:

- PCA
- t-SNE
- UMAP

---

## Overfitting

Overfitting occurs when a model learns the training data too closely, including noise and random variations.

Typical signs:

```text
Training Accuracy = 99%
Validation Accuracy = 72%
```

The model performs very well on training data but poorly on unseen data.

---

## Underfitting

Underfitting occurs when the model is too simple to capture important patterns.

Example:

```text
Training Accuracy = 65%
Validation Accuracy = 63%
```

Both performances are poor.

---

## Bias and Variance

A useful way to understand model behavior is through the bias-variance tradeoff.

```text
High Bias
   ↓
Model too simple
   ↓
Underfitting
```

versus:

```text
High Variance
   ↓
Model too sensitive to training data
   ↓
Overfitting
```

The goal is to find a model that generalizes well to unseen data.

---

## A Simple Scikit-learn Workflow

Python and Scikit-learn provide a convenient workflow for classical machine learning.

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
```

Create some example data:

```python
import numpy as np

X = np.array([
    [1],
    [2],
    [3],
    [4],
    [5]
])

y = np.array([
    2,
    4,
    6,
    8,
    10
])
```

Split the data:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42
)
```

Create the model:

```python
model = LinearRegression()
```

Train it:

```python
model.fit(X_train, y_train)
```

Make predictions:

```python
predictions = model.predict(X_test)
```

Evaluate the model:

```python
mse = mean_squared_error(y_test, predictions)

print("MSE:", mse)
```

The basic pattern is:

```text
Prepare Data
     ↓
Split Data
     ↓
Create Model
     ↓
Fit Model
     ↓
Predict
     ↓
Evaluate
```

---

## Why Train/Test Separation Matters

Suppose we train a model and evaluate it using the same data.

The result might look excellent:

```text
Accuracy = 98%
```

But this does not necessarily mean the model will perform well on new data.

The test set should represent unseen data.

```text
Training Data
      ↓
Model learns
      ↓
New / Unseen Data
      ↓
Model predicts
```

This is what we actually care about in production.

---

## Generalization

Generalization means that a trained model performs well on data it has never seen before.

A useful ML model should not simply memorize the training dataset.

Instead:

```text
Training Examples
       ↓
Learn General Patterns
       ↓
Unseen Examples
       ↓
Useful Predictions
```

This is one of the most important concepts in machine learning.

---

## Classical ML vs Deep Learning

| Classical ML | Deep Learning |
|---|---|
| Often works well with structured/tabular data | Excellent for complex unstructured data |
| Feature engineering often important | Features can be learned automatically |
| Usually smaller datasets | Often benefits from large datasets |
| Scikit-learn commonly used | PyTorch / TensorFlow commonly used |
| Random Forest, SVM, XGBoost | CNN, Transformer, U-Net |

The choice depends on:

- Dataset
- Problem
- Compute resources
- Required performance
- Interpretability
- Deployment requirements

---

## Machine Learning Tools

### Python

Primary programming language for many machine learning workflows.

### NumPy

Numerical computing and array operations.

### Pandas

Data loading, manipulation and analysis.

### Matplotlib

Data visualization.

### Seaborn

Statistical visualization.

### Scikit-learn

Classical machine learning algorithms and utilities.

### XGBoost

Gradient boosting framework.

### PyTorch

Deep learning framework.

### TensorFlow

Deep learning and machine learning framework.

---

## Interview Questions

### What is Machine Learning?

Machine learning is a method of building systems that learn patterns from data and use those patterns to make predictions or decisions.

### What is the difference between supervised and unsupervised learning?

**Supervised learning** uses labeled data where the target is known.

**Unsupervised learning** works with data without predefined target labels and attempts to discover structure or patterns.

### What is overfitting?

Overfitting occurs when a model learns the training data too closely and performs poorly on unseen data.

### What is underfitting?

Underfitting occurs when a model is too simple to capture the underlying patterns in the data.

### Why do we split data into training and testing sets?

The separation allows us to evaluate how well the model generalizes to unseen data.

### What is a feature?

A feature is an input variable used by a machine learning model to make predictions.

### What is a target?

The target is the value the model is trying to predict.

### What is a hyperparameter?

A hyperparameter is a configuration value chosen before or during training that controls how a model or learning algorithm behaves.

---

## Key Takeaways

```text
Machine Learning
       │
       ├── Learn from Data
       ├── Supervised Learning
       ├── Unsupervised Learning
       ├── Reinforcement Learning
       ├── Feature Engineering
       ├── Model Training
       ├── Evaluation
       └── Deployment
```

The important idea is:

> **Good machine learning is not simply about selecting a powerful algorithm.**

It is about building a complete pipeline that transforms raw data into reliable predictions.

```text
DATA
 ↓
PREPARE
 ↓
EXPLORE
 ↓
FEATURES
 ↓
MODEL
 ↓
EVALUATE
 ↓
DEPLOY
```

---

## Lab Takeaway

**Understand the data before choosing the model.**

A sophisticated algorithm cannot compensate for poor data, incorrect features, leakage, inappropriate evaluation, or a badly defined problem.

The foundation of every successful machine learning system is a well-designed end-to-end workflow.
