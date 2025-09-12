---
layout: post
title: "Orbital Mechanics 101: Hohmann Transfer Orbits"
date: 2024-07-31
time: 1642
permalink: posts/HohmannTransfer
tags: general
summary: The simplest way to go from Earth to Mars
published: false
---

## Preface

Over the past year I have been indulging my fascination of how the world works, in particular, the mechanics of space travel.

One of the first things I learned was Hohmann transfer orbits—the simplest way to change orbits.
While this is technically "rocket science", it can be understood with the help of basic physics that most people might have learned in high school.

Let's begin. I will use the example of going from Earth to Mars, because why not!

---

### The Problem

| ![Method 1](/images/fibonacci/M1_cropped.svg){:width="95%"} |
|:--:|
| *You are here image* |

Let's zoom in on the two celestial bodies in question.

| ![Method 1](/images/fibonacci/M1_cropped.svg){:width="95%"} |
|:--:|
| *Earth and Mars* |

---

Simplified, we have to move from the smaller (blue) orbit to the larger (orange) orbit.
The radii of the two are respectively $$r_i$$ and $$a+c$$.
These two radii determine the initial and final velocities.

We can solve this, but eventually we want to orbit Mars.
That


$$ v_i = $$

$$\begin{aligned}
  |\mathbf{A} - \lambda \mathbf{I}| &= 0 \\
  (1-\lambda)(-\lambda) - 1 &= 0 \\
  \lambda^2 - \lambda - 1 &= 0 \\
  \implies \lambda_1 = \frac{1 + \sqrt{5}}{2}&, \quad \lambda_2 = \frac{1 - \sqrt{5}}{2}.
\end{aligned}$$

<hr>

## Wrapping up

I enjoyed this journey.
I hope you did too :)  

Knowledge is power.
Connections between seemingly unrelated topics can unlock ideas that are greater than the sum of its parts!

Stay curious :)

For feedback or discussion, email/Slack or DM me however.


<sub>
**Notes:** <br>
1: I ran the above experiments on a 2021 M1 Pro. The exact numbers may differ on other machines but the trends should remain the same. <br>
6: Thank you Gilbert Strang for writing a wonderful textbook on Linear Algebra :)
</sub>
