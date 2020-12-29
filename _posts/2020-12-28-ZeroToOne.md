---
layout: post
title: "Lessons from Peter Thiel's _Zero to One_"
date: 2020-12-28
time: 2202
permalink: posts/ZeroToOne
tags: general
summary: Interesting research-relevant lessons from Peter Theil's notes on startups and building the future
---

Just finished reading [Zero to One](https://www.goodreads.com/book/show/23346490-zero-to-one) by Peter Thiel. While the book focuses on startups, the observations and advice generalizes surprisingly well to other aspects of life, like research. I was pleasantly surprised that the book had me hooked from cover to cover — unlike most non-fiction books which have one central idea that is re-stated over an absurd number of pages, strewn amidst a plethora of seemingly unrelated anecdotes.

### The main idea
The are two types of progress:
- Copying things that already work, or making existing things slightly better. These are easy to imagine because they already exist. Horizontal progress. Going from **1 to n**.
	- Examples: Taking a typewriter and making 100 of them. _Applying DQN on 57 Atari games._
- Doing something new. **0 to 1**. Much harder to imagine because it doesn't exist.
	- Examples: Taking a typewriter and creating a word processor. _DQN: the idea of using experience replay and target networks when using Q-learning with deep ANNs to make them work._

Apart from this main idea, the following are some more interesting insights.

#### Marketing
- Marketing/Sales/Distribution is important! If you've invented something new but haven't invented an effective way to sell it, you have a bad business no matter how good the product.
- Creating value is not enough, you also have to capture it. Alternatively, if you've invented something new and great but don't have an effective way to sell it, you have a bad business no matter how good the product.
  - _In research we like to think that great ideas will 'sell' themselves; they will stand the test of time. Hopefully. Purists scrunch up their noses at the idea of having to 'sell' their work. But in this world of social media and PR machines, you have to put in some non-trivial effort in getting your work known, or it risks being drowned in obscurity._

#### Teamwork makes the dream work
- Large organizations move slowly and are risk-averse. Lone geniuses can't create industries. Startups operate on the principle that you need to work with people to get stuff done, yet stay small enough so you actually can.
	- _An intuitive one-to-one translation to research. One might have great ideas by yourself, but with a good team one can iterate faster, catalyze off of everyone's strengths. Too many cooks could spoil the broth though, as plenty of time might be wasted in coordinating between large groups. (DeepMind papers beg to disagree, eh? 😛)_
- You're either on the bus or off the bus — you have to be involved full-time on-site to be fully aligned with the mission.
  - _In terms of research, this doesn't mean one shouldn't be involved as secondary members in a project that someone else is leading. It just means not to coast, and to make 100% effective use of the limited time one does spend on that project._
- Like everything else, the foundation should be strong. How well the founders _(or collaborators)_ know each other and how well they work together is critical to success.

#### _Last_ Mover Advantage
- For a company to be valuable, it must grow and endure. Many people focus too much on the short term, losing sight of the long term. Can't really blame them — growth is easy to measure, durability isn't. _Happens a lot in research._
- General characteristics of a monopoly:
	- A proprietary technology: like Google's search. As a general rule of thumb, it has to be atleast an order of magnitude better than the alternatives, otherwise it will probably be perceived as a marginal improvement, hard to see in an already-crowded market.
		- The clearest way to make a 10x improvement is to make something new. 0 to 1. Something valuable where there was nothing before is theoretically an infinite increase in value — a cure to baldness, a drug to eliminate the need for sleep.
		- Or make an existing solution radically better. _For instance, again, DQN._
	- An ability to leverage economies of scale: a new yoga studio can only grow so large, but something like Twitter can grow to enormous scales with the marginal cost to create a new product is spread out over ever greater quantities of users.
- Typical path: Start small and monopolize, then start expanding into related and larger markets. And as you do that, don't disrupt; avoid competition as much as possible; try to play a positive-sum game than a negative-sum game.
- 'First mover advantage' says, if you're the the first to enter a market, you can capture most of it. That's not worth much if someone then comes along and unseats you. Moving first is a tactic, not a goal. You want to be the last mover — to make the last great development in a specific market and enjoy the profits for years. _Replace 'market' with 'research area' and 'profits' with 'impact' or 'citations'._

---

Miscellaneous interesting topics from the book (not directly related to research):

#### Competition vs Monopolies
- Unlike popular opinion, capitalism and competition are opposites: capitalism is premised on the accumulation of capital but under perfect competition all profits get competed away.
- Lies companies tell:
	- Monopolies try to make the world think they are not monopolies, because that only invites unwanted scrutiny, audits, what not. So they are incentivized to exaggerate the competitiveness of their (non-existent) competitors. Example: Google. It has a monopoly on search, but it will present itself as a tech company in which domain its impact is minuscule when compared to its monopoly on search or advertising.
	- Non-monopolists try to make the world think that they are in a league of their own. They try to define their market as an intersection of numerous smaller markets, while monopolies try to define it as a union of numerous smaller markets.
- **[important]** Are monopolies as bad as media paints it to be? On the surface it seems all the profits they are making comes at the expense of society — out of people's wallets! 😱 They deserve their bad reputation in a world where nothing changes — like the _Monopoly_ board game, where the number of properties/deeds are fixed and there is no space for innovations, such as creating a new kind of portable communication devices. But in the real world monopolies create value out of thin air! AT&T created and monopolized the telephone market for decades, followed by IBM's hardware market, Microsoft's OS market, and Apple's mobile-computing market. Monopolies drive progress because the promise of years of monopoly profits enable them to think long-term and invest in ambitious research that firms locked in competition can't dream of.
- "All happy family are alike; each unhappy family is unhappy in its own way." – Tolstoy in _Anna Karenina_. It's the opposite for businesses. All happy businesses are different: they all solved a unique problem; all unhappy businesses are the same: they all failed to escape competition.
- Winning is better than losing, but everyone loses if the war is not worth fighting for...

#### Luck and Control
- People shat on Jack Dorsey for tweeting "Success is never accidental", saying that is how multimillionaire white men think. I guess we can agree there might have been a lot of luck involved<sup>1</sup>, but he also deserves some credit, taking the right steps at the right time. Unfortunately, it's impossible to test how much of success is based on luck – we can't create 1000 copies of the world in 2004 and check how many of them end up having Facebook succeed. _Sad_...
- 4 types of mindset about the future:
	- _Indefinite pessimism_: afraid the future looks bleak, doing nothing about it. The book says Europe is an example because it react to things as they happen and hope they don't get worse.
	- _Definite pessimism_: afraid the future looks bleak, doing something about it. The book gives the example of China, where the middle class is afraid that the definite plans of copying the western world's means to success is unsustainable for the larger population in the long term, and hence are saving every penny for a famine-like future (much like the past).
	- _Definite optimism_: looking forward to the future, making ambitious plans for it. US in the 70s, with long-term programs (such as the space program) which spanned multiple terms, unlike politicians' manifestoes nowadays, which primarily come up with short-term term-length plans.
	- _Indefinite optimism_: believe the future is promising, no idea how to bring about such a future. Rearranging ideas. _The state of AI research today?_

#### The Power Law
- The Power Law is everywhere. 80% of the value is created by 20% of the people/companies/ideas. So everyone needs to think hard about where whatever they want to do falls on the curve. 
	- Even if you're extraordinarily talented, doesn't mean you should start your own company. If you understand the Power Law, you know how tremendously successful you could be by joining the best company while it's growing fast. You could have 100% of the equity of you fully fund your own venture, but if it fails, you'll have 100% of nothing. _Owning just 0.01% of Tesla or DeepMind, in contrast, is incredibly valuable (in all likelihood)._
- What valuable company is no one building? Every correct answer is necessarily a secret: something important and unknown, something hard but doable. If there are many secrets in the world, there are many valuable companies left to build.


That's all from my notes for now. Hope you've learned something out of it too! If you would like to delve more into this, I would definitely recommend reading the book. Or hit me up; I would love to discuss any of this :)

---

<sub>
1: Shout-out to <a href="https://www.youtube.com/watch?v=3LopI4YeC4I&vl=en">this delightful video</a> from Veritasium that makes you think about the unseen effects of luck (good and bad) in your life. Deserves a post of its own..
</sub>
