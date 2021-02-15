---
layout: post
title: "The Mathematics of Love"
date: 2021-02-14
time: 1826
permalink: posts/MathsOfLove
tags: general
summary: How maths can help find a life partner
---

Preface: I first heard about the 37% rule in [Hannah Fry's TED talk](https://www.youtube.com/watch?v=yFVXsjVdvmY). And then again in the book ['Algorithms to Live By'](https://www.goodreads.com/book/show/25666050-algorithms-to-live-by). They both talked about what the rule is, not how or why it works, what assumptions it makes, etc. So I went down that rabbit hole. And what better occasion to write about it than Valentine's Day :)

---

Most of us have or will face the following problem at some point in life: _I really like this person. Are they The One?_

Well, what are the possibilities? Maybe someone else will come along who's even better! So even though you see yourself growing old with your current partner, you jump ship in expectation of things getting even better. But what if you never find someone as amazing as that person (who is now with someone else)... Quite a predicament, eh?

This is what scientists call the "optimal stopping" problem. And this well-studied problem has an optimum solution. **The 37% Rule**. Let's say you've allotted yourself a certain amount of time for finding a partner. (My interpretation of) The rule says that you should spend the first 37% of that time just setting a baseline; meet different people, figure out what you are looking for in a partner, what things you value, what can you compromise on, etc. After the 37% mark, the next person that exceeds the baseline is The One. (this is also known as the Look-Then-Leap strategy)

How come, you ask? Well, buckle yourselves, let's put on our maths h<sup>e</sup>a<sup>r</sup>ts :P

---

Okay, so first of all, let's take a moment to clarify the strategy. Assuming you decide you want to find a within N years, you will spend the first M of those years just figuring out your baseline; you will not settle down with anyone in those M years. And after those M years have passed, the first person you meet who is above the baseline you have set is the best potential partner you've been looking for. You want to know what M is such that the probability of finding the best potential partner is maximized.

Alright, now the problem is clear. Let's move on to the solution.
Let $$P(M,N)$$ denote the probability of meeting the best potential partner after exploring for $$M$$ out of $$N$$ years of the search.

Then:
$$ P(M,N) = \sum_{K={M+1}}^N Pr(\text{meeting the best potential partner in the k-th year}) $$.

Now this inner probability has two components:
- the probability of you bumping into that person in the k<sup>th</sup> year, which is $$\frac{1}{N}$$, and
- the probability of you bumping into the person who set the baseline for this person in the first $$M$$ years, which is $$\frac{M}{K-1}$$.

Putting these together,

$$ \begin{aligned}
	P(M,N) &= \sum_{K=M+1}^N \frac{1}{N} \frac{M}{K-1} \\
	&= \frac{M}{N} \sum_{K=M+1}^N \frac{1}{K-1} \\
	&= \frac{M}{N} \sum_{P=M}^{N-1} \frac{1}{P} \\
	&= \frac{M}{N} \Big( \sum_{P=1}^{N-1} \frac{1}{P} - \sum_{P=1}^{M-1} \frac{1}{P} \Big) \\
	&= \frac{M}{N} \Big( \ln{(N-1)} - \ln{(M-1)} \Big) \\
	P(M,N) &= \frac{M}{N} \Big( \ln{\frac{N-1}{M-1}}\Big) \\
\end{aligned} $$

When this probability is maximized,

$$ \begin{aligned}
\frac{d P(M,N)}{dM} &= 0 \\
\implies \qquad \frac{M}{N} \frac{M-1}{N-1} \frac{-(N-1)}{(M-1)^2} + \ln \frac{N-1}{M-1} \frac{1}{N} &= 0 \\
\ln \frac{N-1}{M-1} &= \frac{M}{M-1}
\end{aligned} $$

Now, for large N<sup>*</sup>,

$$ \begin{aligned}
\ln \frac{N}{M} &\approx 1 \\
\frac{M}{N} &\approx \frac{1}{e} \\
\implies \frac{M}{N} &\approx 0.37
\end{aligned} $$

So the value of M that maximizes the probability of finding the best potential partner after M years of exploration is 37% of the total time of N years that you wanted to settle down in.

Wow, some mathematics has solved your love life, eh? Unfortunately not. There are a bunch of caveats:
- This rule doesn't guarantee that anyone following the rule WILL find their best potential partner with this strategy. Following this rule just maximizes one's probability of finding their best potential partner. In other words, the probability of failing when following the 37% rule is 63%. This might seem like a large number, but it is the smallest failure probability among all the choices of M years you would explore.
- Even if you happen to be lucky enough to find your best potential partner when following this rule, this other person should also feel the same way. For all you know, they are within their 37% time of exploration or something... That's why we've been calling them the best 'potential' partner. It's a two-body problem (pun intended).

Of course, love is a feeling, not exactly something that can be quantified with maths. So take this rule for what it is---a rule of thumb. This was a still a fun exercise, no?

Other examples of the optimal-stopping problem in real life are:
- when approaching a friend's place for dinner by car, at what point should one start looking for parking spots? Settle too early and have to walk far, or wait for too long only to find no good parking spot nearby and hence having to circle back.
- when finding the best person for a job, at what point should one stop interviewing? Settle too early on a not-so-great person, or wait too long only for the good candidates to have taken up jobs elsewhere.
- when to sell a particular stock you are holding?

You get the idea.

Alright, so we know what an optimal strategy in such situations looks like. What kind of strategies do people actually use? Do we follow this 37% Look-Then-Leap rule---by intuition or evolution?

A study in the 90s [1] found that in such situations people roughly found optimal stopping point with a 31% probability, close to the optimal of 37%. More interestingly, they wondered why people leapt earlier than the 37% mark than later? Well, because there is always the _time cost_ of such activities. As time passes, people tend to get bored, or desperate.

---

Anyway, for more such fun connections of real-life problems and commonly-used algorithms, I highly recommend reading the whole book 'Algorithms to Live By'. For instance:
- how to organize your closet, using caching theory,
- how domain hierarchies in nature are a manifestation of sorting,
- how to schedule and actually accomplish the many tasks of a days, using scheduling theory,
- how promotions can work such that people don't stagnate, using networking theory, and so on.

I'll end with a quote from the book:
> And indeed, people are almost always confronting what computer science regards as the hard cases. Up against such hard cases, effective algorithms make assumptions, show a bias toward simpler solutions, trade off the costs of error against the costs of delay, and take chances. These aren’t the concessions we make when we can’t be rational. They’re what being rational means.

<sup>* One might say N is typically not a very large number within human lifespans. But we could consider time in the granularity of months instead of years, and then N can be a reasonably large number for most people. </sup>

---

References:<br>
<sup><sub>[1] Christian, B., & Griffiths, T. (2016). [Algorithms to Live By: The Computer Science of Human Decisions](https://www.goodreads.com/book/show/25666050-algorithms-to-live-by). Macmillan.</sub></sup> <br>
<sup><sub>[2] Fry, H. (2015). The Mathematics of Love. [TED talk](https://www.youtube.com/watch?v=yFVXsjVdvmY).</sub></sup> <br>
<sup><sub>[3] Smith, D. (1997). Solution to the optimal stopping problem [Blog post]. Retrieved from: [https://plus.maths.org/content/solution-optimal-stopping-problem](https://plus.maths.org/content/solution-optimal-stopping-problem)</sub></sup>
