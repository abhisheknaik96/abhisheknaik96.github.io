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

Linear algebra can pop up in unexpected places.
When I was revising material last year for internship interviews, I stumbled upon a beautiful way to compute Fibonacci numbers.
I coded up a few other methods for comparison, starting with naive recursion.

The differences were hilarious.
The linear-algebra approach blows every other method out of the park.

<!-- | ![Exponential computation](/images/fibonacci/M123456_unlabeled.svg){:width="95%"} |
|:--:|
| *Yup, I'm talking about the brown curve.* | -->

Knowledge is power.
Connections between seemingly unrelated topics can unlock ideas that are greater than the sum of its parts!

So let's look at the methods one-by-one.
We'll start with the simplest and work our way up.

Let's get started!

---

#### Method 1: Recursion

The simplest method implements the recursive nature of Fibonacci numbers: $F(n) = F(n-1) + F(n-2)$, with $F(0) = 0$ and $F(1) = 1$.
This is most people's first programming exercise involving recursion.

```python
def fibonacci_recursion(k):
    if k==1:
        return 1
    elif k==0:
        return 0
    else:
        return fibonacci_recursion(k-1) + fibonacci_recursion(k-2)
```

| ![Method 1](/images/fibonacci/M1.svg){:width="95%"} |
|:--:|
| *The x-axis denotes integers and y-axis denotes the time it took for a particular method to compute the N-th fibonacci number. The time per integer is averaged over 500 independent runs of the experiment.* |

Looks like exponential computation complexity, eh?

Note that both `fibonacci_recursion(n)` and `fibonacci_recursion(n-1)` require $Fib(n-2)$, but both compute it independently.

Let's visualize the recursion tree for $F(8)$.
| ![Exponential computation](/images/fibonacci/recursion_tree.png){:width="95%"} |
|:--:|
| *Depiction of the $O(2^n)$ computations for computing $F(n)$* |

The tree grows by a factor of $2$ at each level---exponential complexity. There is so much repeated computation!

---

#### Method 2: Recursion with bookkeeping

An obvious optimization is to store every $F(k)$ that is computed.

```python

def fibonacci_recursion_with_bookkeeping(k):

    # creating a list to store all the computed numbers
    fib_list = np.zeros(k + 1) - 1
    fib_list[0] = 0
    fib_list[1] = 1

    # the recursive function
    def _fibonacci_recursion_with_bookkeeping(k):
        # don't compute the number again if we've computed it once
        if fib_list[k] != -1:
            return fib_list[k]
        else:
            fib_list[k] = _fibonacci_recursion_with_bookkeeping(k-1) + _fibonacci_recursion_with_bookkeeping(k-2)
            return fib_list[k]

    return _fibonacci_recursion_with_bookkeeping(k)
```

| ![Method 1, 2](/images/fibonacci/M12.svg){:width="95%"} |
|:--:|
| *Bookkeeping certainly helps. It cuts down on lots of redundant computation.* |

Method 2 only requires $O(n)$ computation, but the recursive function is called $O(2^n)$ times.

We can do even better by getting rid of the recursion altogether while storing all the results we've computed so far.

---

#### Method 3: Dynamic Programming

```python
def fibonacci_dynamic_programming(k):

    fib_list = np.zeros(k + 1, dtype=int)
    fib_list[1] = 1

    for i in range(2, k+1):
        fib_list[i] = fib_list[i-1] + fib_list[i-2]

    return fib_list[k]
```

| ![Methods 1, 2, 3](/images/fibonacci/M123.svg){:width="95%"} |
|:--:|
| ** |

Pretty good already.

Now let's see how linear algebra comes in.

---

#### Method 4: Matrix power

Let us look at the Fibonacci formula again:
$$\begin{aligned}
  F(k+1) &= F(k) + F(k-1).
\end{aligned}$$
If we consider a vector $x_{k} = [F(k), F(k-1)]^\top$, then $F(k+1) = [1, 1]^\top x_k$.
Similarly, $F(k) = [1, 0]^\top x_k$.
Combining the two,
$$\begin{aligned}
  F(k+1) &= [1, 1]^\top x_k, \\
  F(k) &= [1, 0]^\top x_k, \\
  \text{or}\quad x_{k+1} &= \begin{bmatrix}
      1 & 1 \\
      1 & 0 \\
      \end{bmatrix} x_k.
\end{aligned}$$

Let's call this matrix $A = \begin{bmatrix}
    1 & 1 \\
    1 & 0 \\
    \end{bmatrix}$. Then,

$$\begin{aligned}
  x_{k+1} &= A x_k \\
  &= A A x_{k-1} \\
  &= A^2 x_{k-1} \\
  \implies x_{k+1} &= A^k x_1.
\end{aligned}$$

$F(k)$ is the first element of $x_{k}$, that is, $F(k) = x_{k}[0]$.

So using linear algebra, we can naively compute $F(k)$ by computing the first element of $A^k x_1$.

```python
def fibonacci_linalg_powers(k):
    A = np.array([[1, 1],
                  [1, 0]], dtype=int)
    x = np.array([1, 0])

    A_power = A
    for i in range(2, k):
        A_power = A @ A_power

    return (A_power@x)[0]
```

To compute $F(n)$, this method involves $n$ matrix multiplications. That is, this is also an $O(n)$ method.

| ![Methods 1, 2, 3, 4](/images/fibonacci/M1234.svg){:width="95%"} |
|:--:|
| *The naive matrix-power method is faster than the recursion-based methods, but much slower than dynamic programming.* |

But why compute the powers one by one?

---

#### Method 5: Matrix power by iterative squaring

To compute $A^8$ starting from $A$,
- multiply $AA$ to get $A^2$,
- multiply $A^2 A^2$ to get $A^4$,
- multiply $A^4 A^4$ to get $A^8$.

Just three operations. More precisely, $\log_2(8)$ operations.
Great!

But what about numbers that are not powers of $2$?

Binary representation to the rescue! Recall:
\[ (8)_{10} = (100)_{2} \]

*Lots remaining here.*


```python
def fibonacci_linalg_powers_better(k):
    A = np.array([[1, 1],
                  [1, 0]], dtype=int)
    x = np.array([1, 0])

    A_power = A
    result = np.eye(2, dtype=int)
    while k > 0:
        if k % 2:
            result = result @ A_power
            k -= 1
        k = k // 2
        A_power = A_power @ A_power

    return (result@x)[1]
```

| ![Methods 1, 2, 3, 4, 5](/images/fibonacci/M12345.svg){:width="95%"} |
|:--:|
| *Method 5 of computing matrix powers by iterative squaring is slower than DP for smaller numbers, but the speedup is apparent for larger numbers.* |

Guess what the spiky pattern is?

Answer at the end of the post :) (I don't like leaving things to the reader 'as an exercise'.)

---

#### Method 6: Diagonalization

We're computing matrix powers.
Can we leverage the diagonalization property?
\[ \mathbf{A}^k = \mathbf{S} \Lambda^k \mathbf{S}^{-1} \]

If $\mathbf{A}$ can be diagonalized, instead of computing costly matrix powers, we can just cheaply compute the powers of scalars!

This is clearly desirable. So let's do it.

Recall $\mathbf{S}$ is composed of the eigenvectors of $\mathbf{A}$ arranged row-wise and $\Lambda$ is a diagonal matrix with the corresponding eigenvalues along the diagonal. So we need to compute the eigenvalues and eigenvectors of $\mathbf{A}$.

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

| ![Methods 1, 2, 3, 4, 5, 6](/images/fibonacci/M123456.svg){:width="95%"} |
|:--:|
| *Lol* |

<sub>
About the spiky pattern:
</sub>
