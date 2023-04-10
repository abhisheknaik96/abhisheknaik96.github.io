---
layout: post
title: "Fibonacci numbers with Linear Algebra"
date: 2023-04-10
time: 1440
permalink: posts/FibonnaciLinAlg
tags: general
summary: Using linear algebra to compute Fibonacci numbers efficiently
published: false
---

## Preface



---

### Outline



---

#### Diagonalization

We're computing matrix powers. Can we leverage the following result?
\[ \mathbf{A}^k = \mathbf{S} \Lambda^k \mathbf{S}^{-1} \]

If $\mathbf{A}$ can be diagonalized, instead of computing costly matrix powers, we can just cheaply compute the powers of scalars!

This is clearly desirable. So let's do it.

Recall $\mathbf{S}$ is composed of the eigenvectors of \A arranged row-wise and \L is a diagonal matrix with the corresponding eigenvalues along the diagonal. So we need to compute the eigenvalues and eigenvectors of \A.

First, the eigenvalues:
$$\begin{aligned}
  |\mathbf{A} - \lambda \mathbf{I}| &= 0 \\
  (1-\lambda)(\lambda) - 1 &= 0 \\
  \lambda^2 - \lambda + 1 &= 0 \\
  \implies \lambda_1 = \frac{1 + \sqrt{5}}{2}&, \quad \lambda_2 = \frac{1 - \sqrt{5}}{2}.
\end{aligned}$$

For these eigenvalues, we obtain the eigenvectors: $\mathbf{x}_1 = [\lambda_1, 1]^\top, \mathbf{x}_2 = [\lambda_2, 1]^\top$. Thus,
$$\begin{aligned}
  \mathbf{S} = \begin{bmatrix}
                \lambda_1 & \lambda_2 \\
                1 & 1
                \end{bmatrix}, \quad
  \Lambda = \begin{bmatrix}
              \lambda_1 & 0 \\
              0 & \lambda_2
            \end{bmatrix}.              
\end{aligned}$$
And $\mathbf{S}^{-1} = \frac{1}{\lambda_1 - \lambda_2} \begin{bmatrix}
              1 & -1 \\
              -\lambda_2 & \lambda_1
              \end{bmatrix}$.

So,
$$\begin{aligned}
   \mathbf{A} = \mathbf{S} \Lambda \mathbf{S}^{-1} = \frac{1}{\lambda_1 - \lambda_2}
                 \begin{bmatrix}
                   \lambda_1 & \lambda_2 \\
                   1 & 1
                 \end{bmatrix}
                 \begin{bmatrix}
                   \lambda_1 & 0 \\
                   0 & \lambda_2
                 \end{bmatrix}
                 \begin{bmatrix}
                   1 & -1 \\
                   -\lambda_2 & \lambda_1
                 \end{bmatrix}
\end{aligned}$$

$$\begin{aligned}
   \mathbf{A}^k = \mathbf{S} \Lambda^k \mathbf{S}^{-1} &= \frac{1}{\lambda_1 - \lambda_2}
                 \begin{bmatrix}
                   \lambda_1 & \lambda_2 \\
                   1 & 1
                 \end{bmatrix}
                 \begin{bmatrix}
                   \lambda_1^k & 0 \\
                   0 & \lambda_2^k
                 \end{bmatrix}
                 \begin{bmatrix}
                   1 & -1 \\
                   -\lambda_2 & \lambda_1
                 \end{bmatrix} \\
   &= \frac{1}{\lambda_1 - \lambda_2}
                 \begin{bmatrix}
                   \lambda_1^{k+1} & \lambda_2^{k+1} \\
                   \lambda_1^k & \lambda_2^k
                 \end{bmatrix}
                 \begin{bmatrix}
                   1 & -1 \\
                   -\lambda_2 & \lambda_1
                 \end{bmatrix} \\
   &= \frac{1}{\lambda_1 - \lambda_2}
                 \begin{bmatrix}
                   \lambda_1^{k+1} - \lambda_2^{k+1} & -\lambda_1 \lambda_2 (\lambda_1^k - \lambda_2^k) \\
                   \lambda_1^k - \lambda_2^k & -\lambda_1 \lambda_2 (\lambda_1^{k-1} - \lambda_2^{k-1})
                 \end{bmatrix}
\end{aligned}$$

Now,
$$\begin{aligned}
   \mathbf{A}^k \mathbf{x}
   &= \frac{1}{\lambda_1 - \lambda_2}
                 \begin{bmatrix}
                   \lambda_1^{k+1} - \lambda_2^{k+1} & -\lambda_1 \lambda_2 (\lambda_1^k - \lambda_2^k) \\
                   \lambda_1^k - \lambda_2^k & -\lambda_1 \lambda_2 (\lambda_1^{k-1} - \lambda_2^{k-1})
                 \end{bmatrix} \begin{bmatrix}
                 1 \\
                 0 \end{bmatrix} \\
   &= \frac{1}{\lambda_1 - \lambda_2}
                 \begin{bmatrix}
                   \lambda_1^{k+1} - \lambda_2^{k+1} \\
                   \lambda_1^k - \lambda_2^k
                 \end{bmatrix}.
\end{aligned}$$

The $k$-th Fibonacci number is the integer part of $\mathbf{A} \mathbf{x}[1]$: $F_k = {\huge\lfloor} \dfrac{\lambda_1^k - \lambda_2^k}{\lambda_1 - \lambda_2} {\huge\rfloor}$.

That's it!

The concept of diagonalization has enabled the computation of Fibonacci numbers by simply using powers of scalar numbers.

```python
def fibonacci_linalg_diagonalization(k):
    lambda1 = (1 + np.sqrt(5)) / 2
    lambda2 = (1 - np.sqrt(5)) / 2

    return int((lambda1**k - lambda2**k)/(lambda1 - lambda2))
```

<!-- <sub>
1: Recall that P=NP if finding a solution was as hard as verifying one! But we digress :) -->
