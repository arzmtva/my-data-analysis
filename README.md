# Practical Work 3 — Data Preprocessing Pipeline

## Student Information

**Name:** Gulchoroeva Asema
**Practical Work:** 3
**Variant:** Level B
**Environment:** Google Colab
**Language:** Python

---

## 1. Aim of the Practical Work

The aim of this practical work is to build a data preprocessing pipeline independently and apply different preprocessing strategies to numerical and categorical features.

The work includes:

* classifying features by type;
* handling missing values;
* using different preprocessing strategies for numerical and categorical features;
* creating a `ColumnTransformer`;
* creating a `Pipeline`;
* checking the final dimensions of the training and testing data;
* preventing data leakage during preprocessing.

---

## 2. Dataset

For this practical work, the **Titanic dataset** was used.

The dataset contains information about passengers of the Titanic and includes both numerical and categorical features.

### Target variable

* `Survived` — whether the passenger survived.

### Features used

#### Numerical features

* `Pclass`
* `Age`
* `SibSp`
* `Parch`
* `Fare`

#### Categorical features

* `Sex`
* `Embarked`

The dataset contains missing values, which makes it suitable for demonstrating different missing-value strategies.

---

## 3. Feature Classification

The features were divided into two groups.

| Feature    | Type        | Preprocessing                 |
| ---------- | ----------- | ----------------------------- |
| `Pclass`   | Numerical   | Median + StandardScaler       |
| `Age`      | Numerical   | Median + StandardScaler       |
| `SibSp`    | Numerical   | Median + StandardScaler       |
| `Parch`    | Numerical   | Median + StandardScaler       |
| `Fare`     | Numerical   | Median + StandardScaler       |
| `Sex`      | Categorical | Most frequent + OneHotEncoder |
| `Embarked` | Categorical | Most frequent + OneHotEncoder |

---

## 4. Missing Value Strategy

### Numerical features

For numerical features, the **median** strategy was selected.

The median is suitable because numerical data can contain unusually high or low values. Compared with the mean, the median is less affected by extreme values.

For example, missing values in `Age` are replaced with the median age calculated from the training data.

### Categorical features

For categorical features, the **most frequent** strategy was selected.

This means that a missing categorical value is replaced with the most common category in the training data.

For example, a missing value in `Embarked` is replaced with the most frequently occurring port.

---

## 5. Train/Test Split

The dataset was divided into training and testing sets.

* **80%** of the data was used for training.
* **20%** was used for testing.
* `random_state=42` was used to make the split reproducible.

The preprocessing was fitted only on the training data.

---

## 6. Preprocessing Pipeline

A separate preprocessing pipeline was created for numerical and categorical features.

### Numerical preprocessing

The numerical pipeline contains:

1. `SimpleImputer(strategy="median")`
2. `StandardScaler()`

### Categorical preprocessing

The categorical pipeline contains:

1. `SimpleImputer(strategy="most_frequent")`
2. `OneHotEncoder(handle_unknown="ignore")`

---

## 7. ColumnTransformer

`ColumnTransformer` was used to apply different transformations to different groups of columns.

The numerical features are processed by the numerical pipeline, while categorical features are processed by the categorical pipeline.

```python
preprocessor = ColumnTransformer(
    transformers=[
        ("num", numeric_transformer, numeric_features),
        ("cat", categorical_transformer, categorical_features)
    ]
)
```

This approach allows all preprocessing steps to be organized in one object.

---

## 8. Data Leakage Prevention

To prevent data leakage, the preprocessing was fitted only on the training data.

Correct approach:

```python
X_train_transformed = preprocessor.fit_transform(X_train)

X_test_transformed = preprocessor.transform(X_test)
```

The `fit` operation is performed only on `X_train`.

The test data is processed using `transform` without fitting the preprocessing again.

This prevents information from the test set from influencing the preprocessing parameters.

---

## 9. Final Pipeline

The preprocessing steps were combined into a single pipeline:

```python
model_pipeline = Pipeline(steps=[
    ("preprocessing", preprocessor)
])
```

The pipeline was then applied to the training and testing data:

```python
X_train_final = model_pipeline.fit_transform(X_train)

X_test_final = model_pipeline.transform(X_test)
```

---

## 10. Results

After preprocessing:

* missing values were removed;
* numerical features were standardized;
* categorical features were converted using One-Hot Encoding;
* training and testing data had the same number of final features.

Example result:

```text
Train after Pipeline: (712, 9)
Test after Pipeline: (179, 9)

Dimension of train and test is the same: True
No missing values in train: True
No missing values in test: True
```

The exact number of encoded features may depend on the categories present in the dataset.

---

## 11. Conclusion

In this practical work, a complete preprocessing pipeline was created for the Titanic dataset.

Numerical and categorical features were processed using different strategies. Missing numerical values were filled using the median, while missing categorical values were filled using the most frequent category.

`ColumnTransformer` was used to combine different preprocessing methods, and `Pipeline` was used to organize the preprocessing steps.

The preprocessing was fitted only on the training data and then applied to the test data. This prevents data leakage and ensures that the test data does not influence the training process.

The final check confirmed that the training and testing datasets have the same number of features and that missing values were successfully handled.
