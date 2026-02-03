---
layout: post
title: "Simulating gravity on Earth"
date: 2026-02-02
time: 2116
permalink: posts/ParabolicFlights
tags: general
summary: Parabolic flights: The only way to simulate space on Earth
published: false
---

## Preface


| ![Three bodies revolving around a central body with different angular velocities](/images/lagrange_points/orbits.gif){:width="95%"} |
|:--:|
| Figure 1: Three bodies revolving around a central body with different angular velocities |



---
A ballistic trajectory is a parabola.
$$
\begin{align*} 
\ddot{x}_t &= 0, \\
\ddot{y}_t &= -g. \\
\end{align*}
$$
Integrating and setting the initial conditions:
$$
\begin{align*} 
\dot{x}_t &= v \cos\theta, \\
\dot{y}_t &= -gt + v\sin\theta.
\end{align*}
$$
One more time:
$$
\begin{align*} 
x_t &= v \cos\theta\, t + x_0, \\
y_t &= -\frac{1}{2}gt^2 + v\sin\theta\, t + y_0.
\end{align*}
$$

Substituting $$x_t$$ in $$y_t$$,
$$\begin{align*}
y_t = -\frac{1}{2}g \Big(\frac{x_t - x_0}{v \cos\theta}\Big)^2 + v\sin\theta \Big(\frac{x_t - x_0}{v \cos\theta}\Big) + y_0
\end{align*}$$

Recall from high-school mathematics that the general equation of a parabola is $$ y = ax^2 + bx + c$$.

Hence, we have shown that a thrown object solely under the influence of gravity follows a parabolic trajectory. 

---

## References

1. Gemini gave useful pointers, in particular for the binomial approximation.
