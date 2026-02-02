
# Assignment 2B: k-Nearest Neighbor Classification

## 1. Experimental Design

In this experiment, I implemented the **k-Nearest Neighbor (k-NN)** classification algorithm from scratch, following the specifications in the assignment README.  
No high-level machine learning libraries were used.

Two datasets were evaluated:

- **Lenses Dataset**: A small categorical dataset for contact lens prescription.
- **Credit Approval Dataset**: A real-world dataset with mixed numerical and categorical features and missing values.

### Data Preprocessing

For the Credit Approval dataset, the following preprocessing steps were applied:

1. **Missing Value Imputation**
   - Categorical features: missing values (`?`) were replaced using the **mode** of the feature computed from the training set.
   - Numerical features: missing values were replaced using the **label-conditioned mean** computed from the training set.

2. **Feature Encoding**
   - Categorical features were encoded as integers based on values observed in the training set.

3. **Feature Normalization**
   - Numerical features were normalized using **z-score normalization**, with mean and standard deviation computed from the training data only.

### Distance Function

The k-NN classifier uses:
- **L2 (Euclidean) distance** for numerical features.
- **0/1 mismatch distance** for categorical features.

Predictions are made using **majority voting** among the k nearest neighbors.

---

## 2. Experimental Results

### Lenses Dataset Results

| k | Accuracy |
|---|----------|
| 1 | 1.0000 |
| 3 | 1.0000 |
| 5 | 1.0000 |
| 7 | 1.0000 |

### Credit Approval Dataset Results

| k | Accuracy |
|---|----------|
| 1 | 0.8116 |
| 3 | 0.8478 |
| 5 | 0.8333 |
| 7 | 0.8478 |

---

## 3. Discussion

### Which value of k works best? Why?

From the results, k = 3 and k = 7 achieve the highest accuracy (0.8478) on the Credit Approval dataset. Among them, k = 3 is preferred because it achieves high accuracy while using fewer neighbors, making it less likely to oversmooth class boundaries.

---

### Why does accuracy decrease when k increases?

Increasing k does not guarantee better performance in kNN. As k becomes larger, the classifier considers more distant neighbors, which may belong to different classes. This can introduce incorrect votes and reduce accuracy.

---

### Why does the Lenses dataset achieve perfect accuracy?

The Lenses dataset is very small and has well-separated classes. Because kNN is a memory-based classifier, it can easily memorize the training samples and correctly classify the test samples, resulting in perfect accuracy for all tested values of k.

---

### What did you learn from this exercise?

This exercise helped me better understand how k-Nearest Neighbor works in practice, especially the impact of the parameter k on model performance. By testing different values of k, I observed that accuracy does not necessarily improve monotonically as k increases. 

---

