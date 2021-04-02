---
layout: post
title: "Policy Gradient — some derivations"
date: 2021-04-02
time: 1205
permalink: posts/PolicyGradient
tags: general
summary: Some not-so-obvious derivations of equivalent forms of the policy gradient
---

Preface: There are many equivalent forms of the policy gradient in the literature. As someone relatively new to area of policy-based methods, some of these equivalences or derivations weren't obvious to me, but they seem to be taken for granted in many sources I looked up. So the following is how I explained this to myself, and I'm sure will be useful to someone else as well.

---

Okay, so the two main equivalent forms of the policy gradient are:

$$\begin{aligned}
	\nabla_\mathbb{\theta} J(\mathbb{\theta}) \doteq \mathbb{E}_\tau \Bigg[ G_\tau \sum_{t=0}^{T-1} \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta}(A_t|S_t) \Bigg],
\end{aligned}$$

and

$$\begin{aligned}
	\nabla_\mathbb{\theta} J(\mathbb{\theta}) \doteq \mathbb{E}_\tau \Bigg[ \sum_{t=0}^{T-1} G_t \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta}(A_t|S_t) \Bigg],
\end{aligned}$$

where $$\mathbb{\theta}$$ denotes the parameters of the policy, $$J(\mathbb{\theta})$$ denotes the performance metric (in case of episodic tasks, the expected return from the distribution of starting states), $$\tau$$ represents a trajectory starting from a state sampled from the starting-state distribution $$\mathbb{d}_0$$ and consequently following policy $$\pi_\mathbb{\theta}$$, $$G_\tau$$ denotes the return obtained in trajectory $$\tau$$, $$T$$ denotes the length of such a trajectory, and $$\mathbb{E}_\tau$$ denotes an expectation over trajectories drawn like this.

The difference between the two forms is subtle, yet significant. For a given trajectory, the former uses the full return for every sample update using the trajectory. On the other hand, the latter uses the return per timestep in every update.

The first form, which I call the 'trajectory formulation', is relatively easy to derive, so let's do that first. I first saw this form in Sergey Levine's [Policy Gradient slides](http://rail.eecs.berkeley.edu/deeprlcourse-fa17/f17docs/lecture_4_policy_gradient.pdf) in CS 294-112.

The objective is to maximize the return within episodes starting from any starting state:

$$\begin{aligned}
    J(\mathbb{\theta}) \doteq \mathbb{E}_{\tau \sim \mathbb{d}_0, \pi_\mathbb{\theta}} [ G_\tau ] = \sum_\tau p(\tau) G_\tau,
\end{aligned}$$

where $$p(\tau)$$ denotes the probability of occurrence of trajectory $$\tau$$.

$$\begin{aligned}
    \nabla J(\mathbb{\theta}) &= \sum_\tau \nabla p(\tau) G_\tau \\
		&= \sum_\tau p(\tau) \frac{\nabla p(\tau)}{p(\tau)} G_\tau \\
    &= \mathbb{E}_\tau[G_\tau {\color{blue}{\nabla \ln p(\tau)}}] \qquad \text{(log-likelihood trick)}
\end{aligned}$$

Now, for a trajectory $$\tau = s_0, a_0, r_1, s_1, \ldots, r_T, s_T$$,

$$\begin{aligned}
    p(\tau) &= d_0(s_0) \pi_\mathbb{\theta}(a_0|s_0) p(s_1|s_0,a_0) \pi_\mathbb{\theta}(a_1,s_1) \ldots p(s_T|s_{T-1},a_{T-1}) \\
    \ln p(\tau) &= \ln d_0(s_0) + \sum_{t=0}^{T-1} \ln \pi_\mathbb{\theta}(a_t|s_t) + \sum_{t=0}^{T-1} \ln p(s_{t+1}|s_t,a_t) \\
    {\color{blue}{\nabla_\mathbb{\theta} \ln p(\tau)}} &= \sum_{t=0}^{T-1} \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta}(a_t|s_t)
\end{aligned}$$

Thus,

$$\begin{aligned}
    \nabla J(\mathbb{\theta}) &= \mathbb{E}_{\tau} \big[ G_\tau \sum_{t=0}^{T-1} \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta}(A_t|S_t) \big]
\end{aligned} $$

Cool, we have the first form.

Moving on to the second.

We have been using the gradient of the objective $$ J(\mathbb{\theta}) = \mathbb{E}_\tau [ G_t ] = \mathbb{E}_\tau [\sum_{t=0}^\infty R_{t+1}]$$ everywhere; here we require the gradient of the expected value of a single reward $$\nabla \mathbb{E}_\tau [R_{k}]$$. This is the critical step:

$$\begin{aligned}
    {\color{green}{\nabla \mathbb{E}_\tau [R_{k}] = \mathbb{E}_\tau [ R_k \sum_{t=0}^{k-1} \nabla \ln \pi(A_t|S_t) ]}}.
\end{aligned}$$

This non-intuitive expression is true. Let us verify it for small $$k$$. For $$k=1$$:

$$\begin{aligned}
    \mathbb{E}_\tau[R_1] &= \sum_s \mu(s) \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a)\, r \\
    \nabla \mathbb{E}_\tau[R_1] &= \sum_s \mu(s) \sum_a \nabla \pi(a|s) \sum_{s',r} p(s',r|s,a)\, r \\
    &= \sum_s \mu(s) \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a) \big[ r\, \nabla \ln \pi(a|s) \big] \qquad \text{(log-likelihood trick)}\\
    \nabla \mathbb{E}_\tau[R_1] &= \mathbb{E} \big[ R_1 \nabla \ln \pi(A_0|S_0) \big] \qquad \checkmark
\end{aligned}$$

For $$k=2$$:

$$\begin{aligned}
    \mathbb{E}_\tau[R_2] &= \sum_s \mu(s) \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a) \sum_{s'} \mu(s') \sum_{a'} \pi(a'|s') \sum_{s'',r'} p(s'',r'|s',a')\, r' \\
    \nabla \mathbb{E}_\tau[R_2] &= \sum_s \mu(s) \sum_a {\color{blue}{\nabla \pi(a|s)}} \sum_{s',r} p(s',r|s,a) \sum_{s'} \mu(s') \sum_{a'} \pi(a'|s') \sum_{s'',r'} p(s'',r'|s',a')\, r' \\
    &\quad + \sum_s \mu(s) \sum_a \pi(a|s) \sum_{s',r} p(s',r|s,a) \sum_{s'} \mu(s') \sum_{a'} {\color{blue}{\nabla \pi(a'|s')}} \sum_{s'',r'} p(s'',r'|s',a')\, r' \\
    &= \mathbb{E} \big[ R_2 \nabla \ln \pi(A_0|S_0) \big] + \mathbb{E} \big[ R_2 \nabla \ln \pi(A_1|S_1) \big] \qquad \text{(log-likelihood trick)} \\
    \nabla \mathbb{E}_\tau[R_2] &= \mathbb{E} [ R_2 \sum_{t=0}^{1} \nabla \ln \pi(A_t|S_t) ] \qquad \checkmark
\end{aligned}$$

The second step takes some mental gymnastics to see.

Having verified/obtained the gradient of the expected value of a single reward (the equation in green), the rest of the derivation is straightforward. Summing on both sides of that equation over $$k$$:

$$\begin{aligned}
    \sum_{k=1}^T \nabla \mathbb{E}_\tau [R_{k}] &= \sum_{k=1}^T \mathbb{E}_\tau \Big[ R_k \sum_{t=0}^{k-1} \nabla \ln \pi(A_t|S_t) \Big] \\
    &= \mathbb{E}_\tau \Big[ \sum_{k=1}^T \big( R_k \sum_{t=0}^{k-1} \nabla \ln \pi(A_t|S_t) \big) \Big]
\end{aligned}$$

Consider for a moment that the term $$\nabla \ln \pi\,(A_t \mid S_t)$$ is represented by $$L_t$$. Then the term in the expectations form a triangle as follows (like in [[3]](https://danieltakeshi.github.io/2017/03/28/going-deeper-into-reinforcement-learning-fundamentals-of-policy-gradients)):


| $$R_1 L_0$$ |           |           |               |
|-----------|-----------|-----------|---------------|
| $$R_2 L_0$$ | $$R_2 L_1$$ |           |               |
| $$R_3 L_0$$ | $$R_3 L_1$$ | $$R_3 L_2$$ |               |
| $$\vdots$$  |           | $$\ddots$$  |               |
| $$R_T L_0$$ | $$R_T L_1$$ | $$\dots$$   | $$R_T L_{T-1}$$ |


Re-writing the above sum of terms column-wise instead of row-wise:

$$\begin{aligned}
    \sum_{k=1}^T \nabla \mathbb{E}_\tau [R_{k}] &= \mathbb{E}_\tau \Big[ \sum_{k=1}^T \big( R_k \sum_{t=0}^{k-1} \nabla \ln \pi(A_t|S_t) \big) \Big] \\
    &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \big( \nabla \ln \pi (A_t|S_t) \sum_{k=t+1}^T R_k \big) \Big] \\
    \sum_{k=1}^T \nabla \mathbb{E}_\tau [R_{k}] &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \nabla \ln \pi (A_t|S_t) G_t \Big] \\
    \nabla \mathbb{E}_\tau [\sum_{k=1}^T R_{k}] &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \nabla \ln \pi (A_t|S_t) G_t \Big] \\
    \nabla \mathbb{E}_\tau [G_\tau] &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \nabla \ln \pi (A_t|S_t) G_t \Big] \\
    \implies \nabla_\mathbb{\theta} J(\mathbb{\theta}) &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} G_t \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta} (A_t|S_t) \Big]
\end{aligned}$$

Lo and behold.

The above derivation can be repeated with discounting to obtain the commonly-used form:

$$\begin{aligned}
    \nabla_\mathbb{\theta} J(\mathbb{\theta}) &= \mathbb{E}_\tau \Big[ \sum_{t=0}^{T-1} \gamma^t G_t \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta} (A_t|S_t) \Big].
\end{aligned}$$

Cool. So we've derived the following two equivalent forms of the policy gradient:

$$\begin{aligned}
	\nabla_\mathbb{\theta} J(\mathbb{\theta}) \doteq \mathbb{E}_\tau \Bigg[ G_\tau \sum_{t=0}^{T-1} \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta}(A_t|S_t) \Bigg],
\end{aligned}$$

and

$$\begin{aligned}
	\nabla_\mathbb{\theta} J(\mathbb{\theta}) \doteq \mathbb{E}_\tau \Bigg[ \sum_{t=0}^{T-1} G_t \nabla_\mathbb{\theta} \ln \pi_\mathbb{\theta}(A_t|S_t) \Bigg].
\end{aligned}$$

Hope this helps! Let me know if you spot any typos :)

---

References:<br>
<sup><sub>[1] Levine, S. (2017). [Policy gradient introduction](http://rail.eecs.berkeley.edu/deeprlcourse-fa17/f17docs/lecture_4_policy_gradient.pdf). Lecture slides, CS 294: Deep Reinforcement Learning, UC Berkeley.</sub></sup> <br>
<sup><sub>[2] Schulman, J. (2016). [Optimizing Expectations: From Deep Reinforcement Learning to Stochastic Computation Graphs](http://joschu.net/docs/thesis.pdf), Ph.D. dissertation, UC Berkeley.</sub></sup> <br>
<sup><sub>[3] Seita, D. (2017). Going Deeper Into Reinforcement Learning: Fundamentals of Policy Gradients [Blog post]. Retrieved from: [https://danieltakeshi.github.io/2017/03/28/going-deeper-into-reinforcement-learning-fundamentals-of-policy-gradients](https://danieltakeshi.github.io/2017/03/28/going-deeper-into-reinforcement-learning-fundamentals-of-policy-gradients)</sub></sup> <br>
<sup><sub>[4] Sutton, R.S., Barto, A.G. (2018). [Reinforcement Learning: An Introduction](http://www.incompleteideas.net/book/RLbook2018.pdf), MIT Press.</sub></sup> <br>
