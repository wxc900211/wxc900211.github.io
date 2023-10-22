---
weight: 1
title: "Credit Card Fraud Detection"
subtitle: ""
date: 2023-10-21T21:26:23+02:00
lastmod: 2023-10-21T21:26:23+02:00
draft: false
author: "Xiaochen"
authorLink: ""
description: "Machine Learning in Credit Card Fraud Detection"
license: ""
images: []

tags: ["Prediction"]
categories: ["Machine Learning"]

featuredImage: "https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/CreditCard.png"
featuredImagePreview: "https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/CreditCard_Preview.png"

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: true
ruby: true
fraction: true
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
math:
  enable: false
  # ...
mapbox:
  # ...
share:
  enable: true
  # ...
comment:
  enable: true
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---
<!--more-->

## 1 Description

This dataset contains credit card transactions made by **European cardholders** in the year **2023**. It comprises over **550,000** records, and the data has been anonymized to protect the cardholders' identities. 

{{< admonition tip "Understand Anonymized" >}}
The data is **anonymized** because it is transaction data that contains **PID - Personal Identification details**. And it is European data as well and they have a strong data protection policy - **GDPR**. The data is meant for building **ML models** that can extract patterns from the features, and not for descriptive data analysis whereby the data dictionary would be important.
{{< /admonition >}}

## 2 Key Features

This part is for understading the dataset. Because this data is **anonymized**, it is easier in this case.


* id: Unique identifier for each transaction
* V1-V28: Anonymized features representing various transaction attributes (e.g., time, location, etc.)
* Amount: The transaction amount
* Class: Binary label indicating whether the transaction is fraudulent (1) or not (0)

## 3 Data Cleaning

This part is required before starting building ML models. 

Usually, after the checking, we have to find suitable ways to deal with abnormal data. Because this data is **automatically recoreded from customers' transaction**, the data is very compeleted. 

We can describle this data **"Clean"**.

* Checking for Null Values

```python
creditCard_data.isna().sum()
```

* Checking for Duplicates

```python
creditCard_data[creditCard_data.duplicated()]
```

* Checking for Outliers

```python
## extract the features and the label 
y = creditCard_data.iloc[:,-1]
x = creditCard_data.iloc[:,1:30]
## normalize the data 
scaler = MinMaxScaler()
x_normalized = scaler.fit_transform(x)
sns.boxplot(x = x_normalized)
```
![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/CreditCard_Outlier.png)

## 4 Data Processing

This part is the prepared work before starting building ML models.

* Reducing the dimension of the data 

{{< admonition note "Dimensionality reduction" >}}
The transformation of data from a high-dimensional space into a low-dimensional space so that the low-dimensional representation retains some meaningful properties of the original data, ideally close to its intrinsic dimension.
{{< /admonition >}}

```python
pca = PCA()
pca.fit_transform(x_normalized)
cum_var = np.cumsum(pca.explained_variance_ratio_)
num_components = np.argmax(cum_var >= 0.95)+1
```

The number of components needed to retain 95% of the data is **11**. The algorithm selects the number of components while preserving **95% of the variance** in the data with **11 features**.

* Spiliting into train and test sets

```python
x_train , x_test, y_train , y_test = train_test_split(x_reduced,y,train_size = 0.7)
```
## 5 Apply Models

This business question belongs to 0-1 type (**Classification**). The traditional classifier models we try here is:

* LogisticRegression
* LinearSVC 
* DecisionTreeClassifier
* GaussianNB
* KNeighborsClassifier
* XGBClassifier
* GradientBoostingClassifier
* RandomForestClassifier
* AdaBoostClassifier

```python
models = [
    ('Logistic Regression', LogisticRegression()),
    ('Linear Support Vector Classifier', LinearSVC()),
    ('Decision Tree Classifier', DecisionTreeClassifier()),
    ('Naive Bayes', GaussianNB()),
    ('K-Nearest Neighbors', KNeighborsClassifier()),
    ('XGBoost', XGBClassifier()),
    ('GradientBoostingClassifier', GradientBoostingClassifier()),
    ('RandomForestClassifier', RandomForestClassifier()),
    ('AdaBoostClassifier', AdaBoostClassifier())
    
]
```

```python
results = []
for model_name, model in models:
    model.fit(x_train, y_train)
    y_pred = model.predict(x_test)
    f1 = f1_score(y_test, y_pred)  # Calculate F1-score
    results.append((model_name, f1))
    hyperparameters = model.get_params()
    model_name = model.__class__.__name__
    model_score = f1
    sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
```

```python
model_names = [model[0] for model in sorted_results]
f1_scores = [model[1] for model in sorted_results]

plt.figure(figsize=(8, 4))
bars = plt.barh(model_names, f1_scores, color='green')
plt.xlabel('F1 Score')
plt.title('Model F1 Score Comparison (Descending Order)')
plt.xlim(0, 1)
plt.gca().invert_yaxis()

for bar, score in zip(bars, f1_scores):
    plt.text(score, bar.get_y() + bar.get_height() / 2, f'{score:.2f}', va='center')

plt.show()
```

![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/CreditCard_model.png)

The best one is **RandomForestClassifier**.

## 6 Model Tuning

This part is to find the optimal values of hyperparameters to maximize model performance.

* Using the grid search model.

```python
# Create the parameter grid based on the results of random search 
param_grid = {
    'bootstrap': [True],
    'max_depth': [80, 90, 100],
    'min_samples_leaf': [3, 4],
    'min_samples_split': [8, 10],
    'n_estimators': [100, 200, 500]
}
# Instantiate the grid search model
grid_search = GridSearchCV(estimator = rf, param_grid = param_grid, 
                          cv = 3, n_jobs = -1, verbose = 2)                         
# Fit the grid search to the data
grid_search.fit(x_train, y_train)
best_grid = grid_search.best_estimator_
y_pred = best_grid.predict(x_test)
```
* Creating the confusion matrix

```python
cm = confusion_matrix(y_test, y_pred)
ConfusionMatrixDisplay(confusion_matrix=cm).plot()
```

![](https://raw.githubusercontent.com/wxc900211/PictureBed/main/PicGo/CreditCard_cm.png)

Click [here](https://github.com/wxc900211/DataProject/blob/main/credit-card-fraud-detection.ipynb) to check code in **GitHub**.



