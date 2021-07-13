---
layout: post
title: "Policy Gradient — some derivations (continued)"
date: 2021-07-10
time: 1639
permalink: posts/PolicyGradientDiscounted
tags: invisible
summary: Derivation of discounted policy gradient
---

In the [previous post]({{site.baseurl}}{% link _posts/2021-04-02-PolicyGradient.md %}) we derived the undiscounted policy gradient. Let's derive the discounted version now.

The discounted return is:

$$\begin{aligned}
 J(\mathbb{\theta}) = \mathbb{E}_\tau \big[ G_\tau^\gamma \big] = \mathbb{E}_\tau \Big[ \sum_{k=0}^{T-1} \gamma^t R_{k+1} \Big]
\end{aligned}$$

Taking the derivative:

$$\begin{aligned}
    \nabla J(\mathbb{\theta}) &= \nabla \mathbb{E}_\tau \Big[ \sum_{k=0}^{T-1} \gamma^k R_{t+1} \Big] \\
    &= \sum_{k=0}^{T-1} \gamma^t \nabla \mathbb{E}_\tau \big[ R_{k+1} \big].
\end{aligned}$$


Recall that:
$$\nabla \mathbb{E}_\tau [R_{k}] = \mathbb{E}_\tau \big[ R_k \sum_{t=0}^{k-1} \nabla \ln \pi(A_t|S_t) \big]$$.

$$\begin{aligned}
    \therefore \nabla J(\mathbb{\theta}) &= \sum_{k=0}^{T-1} \gamma^k \mathbb{E}_\tau \big[ R_{k+1} \sum_{t=0}^{k} \nabla \ln \pi(A_t|S_t) \big] \\
    &= \mathbb{E}_\tau \Big[ \sum_{k=0}^{T-1} \big( \gamma^k R_{k+1} \sum_{t=0}^{k} \nabla \ln \pi(A_t|S_t) \big) \Big]
\end{aligned}$$


Like before, representing the terms in the expectation in a triangle:

| $$R_1 L_0$$ |           |           |               |
|-----------|-----------|-----------|---------------|
| $$\gamma R_2 L_0$$ | $$\gamma  R_2 L_1$$ |           |               |
| $$\gamma^2 R_3 L_0$$ | $$\gamma^2 R_3 L_1$$ | $$\gamma ^2 R_3 L_2$$ |               |
| $$\vdots$$  |           | $$\ddots$$  |               |
| $$\gamma^{T-1} R_T L_0$$ | $$\gamma^{T-1} R_T L_1$$ | $$\dots$$   | $$\gamma^{T-1}R_T L_{T-1}$$ |

Re-writing the above sum of terms column-wise instead of row-wise:

$$\begin{aligned}
    \nabla J(\mathbb{\theta}) &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \big( \nabla \ln \pi (A_t|S_t) \sum_{k=t}^{T-1} \gamma^k R_{k+1} \big) \Big] \\
    &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \big( \nabla \ln \pi (A_t|S_t) \gamma^t \sum_{k=t}^{T-1} \gamma^{k-t} R_{k+1} \big) \Big] \\
    &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \nabla \ln \pi (A_t|S_t) \gamma^t G_t \Big] \\
    \implies \nabla J(\mathbb{\theta}) &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \gamma^t G_t \nabla \ln \pi_\mathbb{\theta} (A_t|S_t) \Big]
\end{aligned}$$

Tadaa!

Hope this helps! As usual, do let me know in case you spot any typos.
