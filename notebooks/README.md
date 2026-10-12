# Practical Work 6 — Text Preprocessing and Classification

## Objective
The goal of this practical work is to preprocess text data, convert text into numerical features using TF-IDF, train a classification model, and analyze classification errors.

## Dataset
- **File:** `govt_fund_utilization.csv`
- **Text column:** `Audit_Remarks`
- **Target column:** `Status`

## Tasks Completed
1. Converted text to lowercase.
2. Removed punctuation, digits, and English stop words.
3. Performed tokenization.
4. Applied TF-IDF vectorization with `min_df=2` and `max_df=0.95`.
5. Trained a Logistic Regression classifier.
6. Evaluated model performance using accuracy and a classification report.
7. Analyzed five classification errors and possible reasons for incorrect predictions.

## Results
The initial model achieved an accuracy of approximately **40.32%**. The results showed that some categories were difficult to predict, so the classification errors were analyzed.

## Conclusion
This practical work helped me understand text preprocessing, TF-IDF vectorization, text classification, and model error analysis using Python and scikit-learn.
