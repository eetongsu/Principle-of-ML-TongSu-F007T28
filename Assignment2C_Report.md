
# Assignment 2C: Naive Bayes for Spam Detection

## 1. Experimental Design

In this experiment, we implemented a **Gaussian Naive Bayes classifier from scratch** to perform spam email detection using the Spambase dataset from the UCI Machine Learning Repository.

The dataset contains 4601 email samples with 57 continuous features, including:
- Word frequency features
- Character frequency features
- Capital letter statistics

The task is to classify emails as **spam (1)** or **non-spam (0)**.

### Model Overview

The Naive Bayes classifier is based on Bayes' theorem with the assumption of conditional independence between features.

For continuous features, a **Gaussian distribution** is assumed.  

To avoid numerical underflow, all computations are performed in **log-probability space**.

### Data Splitting

The dataset was randomly split into:
- **80% training set**
- **20% testing set**

All model parameters (class priors, means, and variances) were estimated using only the training data.

---

## 2. Evaluation Results

### Results Table

| Metric     | Value |
|------------|-------|
| Accuracy   | 0.8217 |
| Precision  | 0.7233 |
| Recall     | 0.9385 |
| F1-Score   | 0.8170 |

### Confusion Matrix

```
[[390 140]
 [ 24 366]]
```

---

## 3. Top 5 Discriminative Features

The following features were identified as the most discriminative between spam and non-spam emails based on differences in their distributions between classes:

1. word_freq_your
2. word_freq_remove
3. word_freq_000
4. char_freq_$
5. word_freq_you

### Feature Distribution Visualization

*The following figure compares the distributions of the top 5 features for spam and non-spam emails:*

![Top 5 Feature Distributions](Figure/top_5_features_distribution.png)

---

## 4. Discussion

### 1. Why is Naive Bayes effective for spam detection despite the independence assumption?

Naive Bayes performs well for spam detection because spam emails often contain strong individual indicators, such as specific words, symbols, or capitalization patterns. Even though the independence assumption is not strictly true, Naive Bayes can still achieve good performance as long as these features are conditionally informative. 

---

### 2. What are the limitations of your implementation?

One limitation is the independence assumption, which ignores correlations between features. In real emails, words and characters are often correlated. Additionally, this implementation assumes that all features follow a Gaussian distribution, which may not perfectly match the true distribution of word frequencies.

---

### 3. How could you improve the classifier?

Possible improvements include:
- Using feature selection to remove weak or redundant features. 
- Applying cross-validation to obtain more reliable performance estimates.
- Exploring other variants of Naive Bayes, such as multinomial Naive Bayes, which may better model word frequency data.
- Combining Naive Bayes with more expressive models, such as logistic regression or ensemble methods

---

### 4. What did you learn from this exercise?

This exercise helped me understand how Naive Bayes works in practice and why it is effective for text-based classification problems like spam detection. Implementing the algorithm reinforced my understanding of probability modeling, log-likelihood computation, and evaluation metrics.
