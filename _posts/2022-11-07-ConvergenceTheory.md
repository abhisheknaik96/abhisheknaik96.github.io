---
layout: post
title: "Essentials of Convergence Theory for RL"
date: 2022-11-07
time: 1810
permalink: posts/ConvergenceTheory
tags: general
summary: Deciphering what it takes to prove the convergence of RL algorithms
published: false
---

## Preface

Given an RL algorithm, how can you tell if it converges? If it does converge, where to? And what is the quality of the solution?

_"checking the positive definiteness of the key 'A' matrix", "the system is stable because the real parts of the eigenvalues of the matrix are all positive"_

I could not quite comprehend what the phrases like the above meant when while reading papers and textbooks.
Having spent some time understanding them now, it turns out convergence theory is quite straightforward.
In this post I will try to explain the basic concepts so you also have a better understanding of the theoretical foundations of the algorithms that we use on an everyday basis.

---

### Outline

- General form of common algorithms
- The underlying linear algebra

---

Consider the update rule for TD learning for episodic problems with the total-reward criterion:

$$\begin{aligned}
  V_{t+1}(S_t) \doteq V_t(S_t) + \alpha \Big( R_{t+1} + V_t(S_{t+1}) - V_t(S_t) \Big),
\end{aligned}$$
where $S_t$ denotes the state at time $t$, $V_t(S_t)$ denotes the value estimate of that state at time $t$, $R_{t+1}$ denotes the sampled reward at time step $t+1$, and $\alpha$ denotes the step size.

TD learning with linear function approximation ($v_\pi(s) \sim w^\top x(s)$):
$$\begin{aligned}
  w_{t+1} \doteq w_t + \alpha\, ( R_{t+1} + w_t^\top x_{t+1} - w_t^\top x_t )\, x_t,
\end{aligned}$$
where $w \in \mathbb{R}^d$ are the learnable parameters and $x_t$ denotes the feature vector at time step $t$.

Several machine-learning algorithms have an update of the following form:

$$\begin{aligned}
	u_{t+1} &\doteq u_t + \alpha (A_t u_t + b_t) \\
  \text{or} \qquad \nabla u_t &\propto A_t u_t + b_t
\end{aligned}$$

(ToDo: make the vectors and matrices bold)

In case of TD learning, $u_{t} = w_t$, $b_t = x_t R_{t+1}$, $A_t = x_t (x_{t+1} - x_t)^\top$.

The asymptotic behavior of the sequences generated by the algorithms of the above kind mimics that of an ordinary differential equation (ODE) of the kind: (ToDo: can be clearer about this)

$$\begin{aligned}
	\frac{d u_t}{dt} = A u_t + b.
\end{aligned}$$

To analyse this connection between the linear-algebra form of the updates with ODEs, let us consider a simpler ODE first:
$$\begin{aligned}
	\frac{d u_t}{dt} = A u_t,
\end{aligned}$$
where $u_t$ is a vector in $\mathbb{R}^d$ and $A$ is a matrix in $\mathbb{R}^{d\times d}$.

Let us take a further step back and analyze an even simpler case first.
Let $u_t$ be a scalar. Then $A$ is a scalar as well, which we now denote by $a$:
$$\begin{aligned}
	\frac{d u_t}{dt} = a u_t,
\end{aligned}$$

Solving for $u_t$,
$$\begin{aligned}
	\int \frac{d u_t}{u_t} &= \int a d_t \\
  \ln u_t &= at + c \\
  u_t &= k e^{at}.
\end{aligned}$$
At $t=0$, if we began with $u_0$, then:
Solving for $u_t$,
$$\begin{aligned}
  u_t &= u_0 e^{at}.
\end{aligned}$$

<!-- So $\frac{d u_t}{dt} = a u_t$ has a solution $u_t = C e^{at}$.
We can compute the scalar $C$ from the initial conditions (at $t=0$): $u_0 = C$. -->

<!-- Instead of solving for $u_t$, we will verify a potential solution of $u_t$.$^1$  -->

Now let's go back to the case where $u_t$ is a vector and we have the matrix $A$. Then the solution for $\frac{du}{dt} = A u_t$ is:
$$\begin{aligned}
  u_t &= u_0 e^{At}.
\end{aligned}$$
What is $e^{At}$, you may ask. Recall the definition of the exponential function:
$$\begin{aligned}
  e^x \doteq 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots
\end{aligned}$$
Similarly, when the exponent is a matrix,
$$\begin{aligned}
  e^{At} \doteq I + At + \frac{(At)^2}{2!} + \frac{(At)^3}{3!} + \cdots
\end{aligned}$$

Now, if the matrix $A$ is diagonalizable we can massively simplify the expression of $e^{At}$.

(ToDo: think about what to say about diagonalizability).

Let's see how. $A$ being means diagonalizable we can write $A = S \Lambda S^{-1}$, where the columns of $S$ are the eigenvectors of $A$ and the elements of the diagonal matrix $\Lambda$ are the eigenvalues of $A$.
Then,
$$\begin{aligned}
  e^{At} &= 1 + S \Lambda S^{-1} t + \frac{(S \Lambda S^{-1} t)^2}{2!} + \frac{(S \Lambda S^{-1} t)^3}{3!} + \cdots \\
  &= 1 + S \Lambda S^{-1} t + \frac{(S \Lambda S^{-1} t)(S \Lambda S^{-1} t)}{2!} + \cdots \\
  &= 1 + S \Lambda S^{-1} t + \frac{(S \Lambda^2 S^{-1} t^2)}{2!} + \frac{(S \Lambda^3 S^{-1} t^3)}{3!} \cdots \\
  &= S \big[ I + \Lambda t + \frac{\Lambda^2 t^2}{2!} + \frac{\Lambda^3 t^3}{3!} + \cdots \big] S^{-1} \\
  e^{At} &= S e^{\Lambda t} S^{-1}.
\end{aligned}$$

So if $A$ is diagonalizable, then the solution of $\frac{du}{dt} = Au$ can be written as:
$$\begin{aligned}
  u_t &= S e^{\Lambda t} S^{-1} u_0.
\end{aligned}$$

From this matrix expression we can obtain another expression involving only vectors.
That is the expression which makes it obvious how the eigenvalues affect the stability of an algorithm.

Final derivation now, I promise.
Recall that the set of eigenvectors in $S$ form a basis.
That means $u_0 = Sc$ for some $c \in \mathbb{R}^d$.
So,

$$\begin{aligned}
  u_t &= S e^{\Lambda t} c \\
  &= \begin{bmatrix}
      | &  & | \\
      x_1 & \cdots & x_d \\
      | &  & |
      \end{bmatrix} \begin{bmatrix}
      e^{\lambda_1 t} & \cdots & 0 \\
      \vdots & \ddots & \vdots\\
      0 & \cdots & e^{\lambda_d t}
      \end{bmatrix} \begin{bmatrix}
      c_1 \\
      \vdots \\
      c_d
    \end{bmatrix} \\
  &= \begin{bmatrix}
      | &  & | \\
      x_1 & \cdots & x_d \\
      | &  & |
      \end{bmatrix} \begin{bmatrix}
      c_1 e^{\lambda_1 t}\\
      \vdots \\
      c_d e^{\lambda_d t}
    \end{bmatrix} \\
  u_t &= c_1 e^{\lambda_1 t} x_1 + \ldots + c_d e^{\lambda_d t} x_d.
\end{aligned}$$

There.
Let's look at it closely:
$$\begin{aligned}
  u_t &= c_1 e^{\lambda_1 t} x_1 + \ldots + c_d e^{\lambda_d t} x_d.
\end{aligned}$$

We are interested in the asymptotic solution, as in, when $t\to \infty$.
- What happens when any $\lambda_i$ is positive? Divergence.
- What happens when all $\lambda_i < 0$? Convergence. And to zero.
- What happens when some $\lambda_i < 0$ and one or more are zero?
<br>

(ToDo: make the final point after learning more about the case of complex numbers.)

<!-- <sub>
1: Recall that P=NP if finding a solution was as hard as verifying one! But we digress :) -->
