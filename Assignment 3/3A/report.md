# Sparse Autoencoder

## 1. Overview

In this assignment, I implemented a sparse autoencoder including:

- Random patch sampling from natural images  
- Forward propagation  
- Cost function computation (reconstruction error + l2 decay + sparsity penalty)  
- Backpropagation with sparsity constraint  
- Numerical gradient checking  

---

## 2. Obstacles Faced

### (1) Understanding the Sparsity Penalty

The most challenging part was correctly implementing the KL-divergence sparsity term:

\[
\text{KL}(\rho \| \hat{\rho})
\]

Key difficulties:
- Computing the average hidden activation correctly (`rho_hat`)
- Making sure the sparsity term is added only to the hidden layer error during backpropagation
- Avoiding shape mismatches when adding the sparsity term

---

### (2) Backpropagation with Sparsity

Initially, I made mistakes when adding the sparsity derivative into cost function.

The correct approach was:
- Compute the reconstruction error
- Compute sparsity gradient term
- Add sparsity term before applying sigmoid derivative

I carefully checked dimensions at each step to ensure consistency.

---

### (3) Numerical Gradient Checking

While implementing `computeNumericalGradient`, I encountered issues with:
- Copying parameter vectors correctly
- Extracting only the cost when the function returned

I resolved this by:
- Creating explicit copies of `theta`
- Writing a small helper function to extract only the cost value

After fixing this, the analytical and numerical gradients matched closely, confirming correctness.

---

## 3. Conclusion

The main difficulties were correctly incorporating the sparsity penalty into both the cost function and backpropagation. After carefully reviewing the mathematical formulas and checking tensor dimensions, the implementation worked as expected.
