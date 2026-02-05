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

If the aircraft was in vacuum and turned its engines off, then it would follow a parabolic trajectory as we showed above.

However, the aircraft is travelling through Earth's atmosphere and will experience a drag force opposite to the direction of motion. So if the aircraft's engines are turned off, then the aircraft would experience a deceleration (causing the passengers to be thrown towards the cabin---much like when a bus brakes/decelerates hard and standing passengers are thrown towards the front). 

To counter the deceleration caused by atmospheric drag, the aircraft needs to keep its engines on---just enough to balance the atmospheric drag. If the thrust provided by the engine is greater than the drag, then the passengers will be pushed into their seats; if the thrust is lower than the drag, they will be pushed forward.

Turns out that controlling the thrust requires precise control by the pilots because the atmospheric drag changes continuously due to the changing velocity and height of the aircraft.

<plot of thrust w.r.t. time/distance>

I used the following equation for drag:
$$\begin{align*}
F_d \doteq \frac{1}{2}\rho v^2 c_d A,
\end{align*}$$
where, $$F_d$$ denotes the drag force, $$\rho$$ denotes the density of the fluid in which the object is traveling, $$v$$ denotes the velocity of the object w.r.t. the fluid, $$c_d$$ is the drag coefficient, and $$A$$ denotes the surface area of the object in the direction of motion.

Note that while the aircraft is in the free-fall phase, its velocity changes---changing the drag. The drag depends on the square of the velocity. And the square of the velocity is linear in the square of time, implying the drag depends on the squre of time, or that the drag has a parabolic shape w.r.t. time, just as we see in the above plot.

---

What angle should the aircraft be going at when starting the free fall?

45 degrees? That's the first angle that comes to mind, right? At least it did for me, because it rang a faint bell that throwing a ball at 45 degrees maximizes the distance<sup>2</sup>. But maximizing the distance is not the objective here, is it? 

What is the objective? Maximizing the amount of _time_ of the weightlessness feeling sounds like a reasonable guess. It would enable performing as many experiments as possible, to get used to being in space, etc. 

Let's compare the time of flight versus the angle of approach. 

<plot>

Clearly, the larger the angle, the larger the time of flight. So should the aircraft start the weightlessness phase at 90 degrees? 

Not really. We haven't accounted for an critical component so far: the g forces experienced by the aircraft and its passengers during the pull-up and pull-out phases. 

Let's consider the g forces felt for different angles of approach. 

<plot>

Modern commerical aircrafts are designed to withstand g forces up to +2.5g. _[find a good reference from this [Wikipedia section](https://en.wikipedia.org/wiki/Load_factor_(aeronautics)#Design_standards)]_

Aircrafts for parabolic flights are retrofitted to withstand more g forces that a commercial aircraft, but the next consideration is also how much g forces the human passengers can handle. Fighter pilots can withstand up to 9g for a few seconds; F1 drivers can _[get references]_; regular folks like you and me may not be comfortable beyond 2g. 

So that is the next tradeoff. Comfort of the passengers versus time of weightlessness. The sweet spot is considered to be 2g. This translates to an angle of about __.

The angle of __ implies a radius of curvature of ___. An astute reader may note that the centripetal force will only completely add with the gravity at the bottom of the arc. The total g force decreases from the starting point to the end of the arc. We know that this makes the ride more comfortable, but for higher fuel efficiency, it makes sense to decrease the radius of curvature and reach the angle of approach faster. If the pilots are to maintain a g force of 2g on the passengers and the aircraft, the radius of curvature needs to get tighter and tighter from the starting of the arc.

<sketch of the arc with some circles for visualization. The circles start large and become smaller>

---

## References

1. Gemini gave useful pointers, in particular for the binomial approximation.
2. Growing up, my friends and I certainly learned that through trial-and-error while playing cricket.
