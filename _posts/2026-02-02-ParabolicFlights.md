---
layout: post
title: "Parabolic flights: Simulating microgravity on Earth"
date: 2026-02-02
time: 2116
permalink: posts/ParabolicFlights
tags: general
summary: Understanding he only way to simulate space on Earth
published: false
---

## Preface





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

| ![Visualization of the thrust required from airplan engines to negate the atmospheric drag](/images/parabolic_flight/thrust_to_negate_drag.png){:width="95%"} |
|:--:|
| Figure X: Thrust required by the airplane engines to negate atmospheric drag. |

I used the following equation for drag:
$$\begin{align*}
F_d \doteq \frac{1}{2}\rho v^2 c_d A,
\end{align*}$$
where, $$F_d$$ denotes the drag force, $$\rho$$ denotes the density of the fluid in which the object is traveling, $$v$$ denotes the velocity of the object w.r.t. the fluid, $$c_d$$ is the drag coefficient, and $$A$$ denotes the surface area of the object in the direction of motion.

Note that while the aircraft is in the free-fall phase, its velocity changes---changing the drag. The drag depends on the square of the velocity. And the square of the velocity is linear in the square of time, implying the drag depends on the squre of time, or that the drag has a parabolic shape w.r.t. time, just as we see in the above plot.

Here is the code to generate the above plot:
```python
import jax.numpy as jnp
import matplotlib.pyplot as plt

# Constants
g = 9.81           # m/s^2
m = 10_000         # Mass of an empty Falcon 20 + crew + researchers + equipment (kg; 7500+~2500)
Cd = 0.02          # Drag coefficient
A = 41             # Wing area (m^2; for aircrafts, wing area is considered instead of frontal area)
rho_0 = 1.225      # Air density at sea level (kg/m^3)
H_scale = 10400    # Scale height for density (m)

# Initial conditions for the 20-second parabola
v_0 = 160           # Starting velocity (m/s)
y_0 = 8000          # Initial altitude (m)
theta = jnp.deg2rad(45)  # Entry angle

duration = 23      # simulation duration
dt = 0.05
t = jnp.arange(0, duration, dt)

def simulate_parabola(t, v_0, y_0, theta):
    # velocity components under projectile motion (assuming drag is negated)
    v_x = v_0 * jnp.cos(theta)
    v_y = v_0 * jnp.sin(theta) - g * t
    v_mag = jnp.sqrt(v_x**2 + v_y**2)

    # distance and altitude
    x = v_0 * jnp.cos(theta) * t
    y = y_0 + (v_0 * jnp.sin(theta) * t) - (0.5 * g * t**2)

    # air density: rho(h) = rho_0 * exp(-h / H_scale)
    rho = rho_0 * jnp.exp(-y / H_scale)

    # thrust required is equal and opposite to drag: 0.5 * rho * v^2 * Cd * A
    thrust = 0.5 * rho * (v_mag**2) * Cd * A

    return x, y, thrust

x, y, thrust = simulate_parabola(t, v_0, y_0, theta)

# plotting the results
fig, ax1 = plt.subplots(figsize=(10, 5))

# plot trajectory
ax1.set_xlabel('Horizontal Distance (m)')
ax1.set_ylabel('Altitude (m)', color='tab:blue', rotation=0, labelpad=40)
ax1.plot(x, y, color='tab:blue', linewidth=3)#, label='Flight Path')
ax1.tick_params(axis='y')
ax1.grid(True, linestyle='--', alpha=0.7)
ax1.spines[['top']].set_visible(False)

# plot required thrust
ax2 = ax1.twinx()
ax2.set_ylabel('Required\nThrust (kN)', color='tab:orange', rotation=0, labelpad=40)
ax2.plot(x, thrust / 1000, color='tab:orange', linestyle='--')#, label='Thrust to Negate Drag')
ax2.tick_params(axis='y')
ax2.spines[['top']].set_visible(False)

fig.tight_layout()
plt.show()
```

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

| ![Visualization of net g force in a circular pull-up arc](/images/parabolic_flight/pull_up_circular.png){:width="95%"} |
|:--:|
| Figure X: In a circular pull-up arc, the net downward g force w.r.t. the aircraft and its passengers is 2g is only at the beginning of the arc and keeps decreasing as the angle of the aircraft increases. |

| ![Visualization of net g force in a circular pull-up arc](/images/parabolic_flight/pull_up_2g.png){:width="95%"} |
|:--:|
| Figure X: The aircraft can take a sharper turn while maintaining a net g force of 2g and attain the desired angle of entry into the weightlessness phase. |

---

## References

1. Gemini gave useful pointers, in particular for the binomial approximation.
2. Growing up, my friends and I certainly learned that through trial-and-error while playing cricket.
3. https://www.grc.nasa.gov/www/k-12/VirtualAero/BottleRocket/airplane/drageq.html