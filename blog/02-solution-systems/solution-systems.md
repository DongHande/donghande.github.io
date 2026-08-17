---
layout: post
title: "Intelligence Beyond Models: The Evolution of Solution Systems"
date: 2026-08-17
description: "A framework for understanding how search, AI models, and mechanisms work together, shift as problems mature, and expand the frontier of solvable problems."
permalink: /blog/02-solution-systems/
---

[中文版 →](/blog/02-solution-systems-zh/)

Over the past few years, progress in AI has often been equated with progress in models: larger models, longer context windows, stronger reasoning, and higher benchmark scores.

But a model is only one part of an intelligent system.

Once the focus shifts from how capable a model is to how a system solves a problem, a more fundamental question appears:

> **How can a problem be solved with sufficiently low uncertainty under a finite computational budget?**

From this perspective, search, AI models, and mechanisms are three fundamental ways of solving problems. They work together, and as a problem matures, the solution system moves toward a more effective tradeoff between computation and uncertainty.

---

## I. Computational Cost and Uncertainty

Suppose an algorithm can solve a problem with perfect accuracy but requires $$10^{100}$$ operations.

It is not a usable solution.

Conversely, a method that requires almost no computation but offers no basis for trusting its result is not very useful either.

A solution system therefore has at least two fundamental properties:

$$
\boxed{C=\text{computational cost}}
$$

and

$$
\boxed{U=\text{uncertainty}}
$$

An intelligent system is not simply looking for

$$
\min U
$$

but for a better

$$
\boxed{(C,U)}
$$

combination.

Error and uncertainty need to be distinguished here.

Let $$W$$ denote the real world and $$S$$ a solution system. There is some actual discrepancy between them:

$$
E=D(S,W)
$$

That discrepancy is real, but because the world cannot be observed completely, $$E$$ is usually not known directly. What can be obtained from data, experiments, and experience is an estimate of it:

$$
p(E\mid \mathcal D)
$$

> **Error is the real but unknown discrepancy between a solution and the world; uncertainty is our knowledge of that discrepancy.**

An intelligent system must determine whether its current uncertainty is already low enough and, if not, how much additional computation is justified.

Intelligence is therefore, from the outset, a problem of jointly optimizing computational cost and uncertainty.

---

## II. Three Fundamental Ways to Solve Problems

There are three broad ways to improve the computation–uncertainty tradeoff:

$$
\boxed{
\text{search}
\qquad
\text{AI models}
\qquad
\text{mechanisms}
}
$$

They are not competing paths. They are three different arrangements of where computation happens and how uncertainty is reduced:

- **Search:** spends substantial and variable computation online, acquiring new information through experimentation, execution, and verification to reduce uncertainty about the current problem.
- **AI models:** move much of the computation into training, compressing historical experience into parameters so that predictions require less online computation, while retaining uncertainty from statistics and distribution shift.
- **Mechanisms:** execute explicitly specified mappings with costs that are usually more stable and predictable; uncertainty is lowest within their valid domain but concentrated in whether the rules and boundary conditions are correct.

The distinction is not a simple progression from high cost to low cost. Each method has a different computation–uncertainty structure.

### Search

Search explores alternatives and acquires new information from the environment, simulation, execution, or verification. It places computation after a problem arrives: deeper search and broader branching increase online cost, while richer feedback and more reliable verification generally reduce uncertainty about the current case.

In Go, for example, a system can expand several candidate moves from the current state:

$$
s
\rightarrow
a_1,a_2,\ldots,a_n
\rightarrow
\text{different future outcomes}
$$

Software development can follow a similar loop:

$$
\text{modify}
\rightarrow
\text{run}
\rightarrow
\text{observe}
\rightarrow
\text{modify again}
$$

The essence of search is:

> **Spend computation at decision time to acquire new information and reduce uncertainty.**

Search can adapt to new problems with little prior experience, but its cost can grow rapidly with the search space. Whether uncertainty actually falls depends on the environment providing informative feedback.

### AI Models

When extensive experience with a class of problems already exists, there is no need to search from scratch every time. AI models compress the cost of past search and learning into a forward pass.

Given historical data

$$
(x_1,y_1),\ldots,(x_n,y_n)
$$

a system can learn

$$
f_\theta(x)\approx y
$$

and then produce a prediction directly for a new input.

In Go, what might otherwise require expanding a long sequence of future positions

$$
s_t
\rightarrow
a_t
\rightarrow
s_{t+1}
\rightarrow
\cdots
\rightarrow
R
$$

can instead be estimated by a learned value function

$$
V_\theta(s)
$$

or a policy

$$
\pi_\theta(a\mid s)
$$

Training an AI model may be expensive, but that cost can be amortized across many later uses, making online inference far cheaper than repeating the original search. The tradeoff is that the output remains a statistical estimate: incomplete data coverage, distribution shift, and miscalibration all preserve uncertainty.

The core exchange made by an AI model is therefore:

> **Use historical experience and offline computation to reduce online computation, while accepting statistical uncertainty that can be estimated but not eliminated entirely.**

### Mechanisms

For some problems, the mapping from input to output can be specified explicitly:

$$
y=f(x)
$$

Arithmetic algorithms, compilers, database transactions, network protocols, and deterministic programs are examples.

This mode of solving problems can be called a **mechanism**.

A mechanism still requires computation, but it executes an explicitly specified mapping rather than statistically estimating one. Its computational cost is not necessarily always the lowest, but it is usually more stable, analyzable, and reusable.

When the rules are correct and their assumptions hold, a mechanism has the lowest uncertainty of the three. Its remaining uncertainty sits outside the execution itself: whether the specification is correct, whether the input satisfies its assumptions, and whether the environment has changed. Beyond its valid domain, deterministic execution can produce the wrong result with perfect consistency.

The defining property of a mechanism is therefore:

> **Within an explicit boundary, use predictable computation to produce stable results, concentrating uncertainty in the rules and boundary conditions themselves.**

---

## III. Coordination and the Rightward Shift of Solution Systems

Real solution systems rarely choose only one of the three. They combine them.

Better AI models make search more efficient:

$$
\text{AI models}
\rightarrow
\text{more efficient search}
$$

The experience produced by search improves models:

$$
\text{search}
\rightarrow
\text{new experience}
\rightarrow
\text{better AI models}
$$

Mechanisms provide reliable execution and verification:

$$
\text{mechanisms}
\rightarrow
\text{execution and verification}
\rightarrow
\text{better search}
$$

The more accurate picture is therefore:

$$
\boxed{
\text{search}
\leftrightarrow
\text{AI models}
\leftrightarrow
\text{mechanisms}
}
$$

Together, the three form a solution system and improve its tradeoff between computation and uncertainty. Coordination describes how they combine at a given moment; a rightward shift describes how that combination changes as a problem matures.

Although all three modes remain present, the center of gravity of a solution system tends to change as a problem domain matures:

$$
\boxed{
\text{search-dominant}
\rightarrow
\text{model-dominant}
\rightarrow
\text{mechanism-dominant}
}
$$

This transition can be called the **rightward shift of a solution system**.

A new problem usually lacks sufficient prior experience, so it demands extensive search, experimentation, and verification. As experience accumulates, AI models can make more of the required judgments directly. Once some of the underlying regularities are understood, formalized, and validated, they can be crystallized into explicit mechanisms.

A rightward shift does not mean that search or models disappear. It means:

> **For the same problem domain, the solution system continually reorganizes itself to achieve a better combination of computational cost and uncertainty.**

Go provides a clear example. An unfamiliar position may require extensive search. Stronger policy and value models eliminate unproductive branches. Some fully solved local positions can be handled by a deterministic policy or a lookup table.

Software engineering follows a similar pattern. An unfamiliar failure may initially require repeated modification, execution, and observation. Once enough similar experience has accumulated, an AI model can propose a good solution directly. Stable and recurring cases may eventually become compiler checks, static-analysis rules, libraries, or interfaces.

A problem that required repeated search yesterday may require only a mechanism call today.

Science and engineering have developed in much the same way. Many problems begin with experimentation and trial and error, then acquire empirical models, and finally yield stable regularities that become formulas, algorithms, machines, and infrastructure.

Seen this way, a central process of civilization is giving more and more problems solution systems that sit farther to the right.

---

## IV. Rightward Shifts and the Expansion of the Problem Frontier

Within a fixed problem domain, progress appears as a rightward shift in its solution system. Across an entire intelligent system, the more consequential change is:

$$
\boxed{\text{the frontier of solvable problems expands}}
$$

The two processes are connected.

Modern software development rests on processors, operating systems, compilers, networks, databases, and software libraries. Because these lower-level problems already have mature solutions, developers do not need to rediscover integer representation or reimplement network protocols. Their limited computational and cognitive resources can be directed toward larger problems.

The same is true for AI systems. Reliable code executors, databases, calculators, and other tools allow them to explore higher-level problems without repeatedly solving the underlying capabilities.

Progress in intelligence therefore has two simultaneous directions:

$$
\boxed{\text{existing problems shift rightward}}
$$

and

$$
\boxed{\text{the frontier of new problems expands outward}}
$$

At the new frontier, systems still depend more heavily on search because experience is scarce and mature mechanisms do not yet exist. But this search does not begin from zero. It builds on models and mechanisms produced by earlier rightward shifts.

The rightward shift of existing problems therefore makes the exploration of new ones more efficient.

This also leads to a more complete definition of progress in intelligence.

Within this framework, intelligence cannot be reduced to model capability or defined simply as uncertainty reduction.

A more complete description is:

> **Intelligence is the capacity to discover and organize better solution systems under the joint constraints of computation and uncertainty.**

Its progress consists of two processes: existing solutions move rightward, and the frontier of solvable problems expands.

These processes reinforce one another. Rightward shifts solve existing problems more effectively; the computation they free and the capabilities they accumulate let the system enter a larger unknown space.

Progress in intelligence is therefore not simply a matter of building increasingly capable models. A more accurate picture is:

$$
\boxed{
\text{search}
\leftrightarrow
\text{AI models}
\leftrightarrow
\text{mechanisms}
}
$$

For one problem after another, the center of gravity of the solution moves rightward, while the boundary of what the overall system can address moves outward.

---

## V. Implications for the Next Generation of AI

AI development today often assumes an implicit equivalence:

$$
\text{stronger AI}
\approx
\text{stronger AI models}
$$

But an AI model is only one component of a complete intelligent architecture.

A stronger system must coordinate search, AI models, and mechanisms, choosing among them according to the computational cost and uncertainty of the problem at hand. Some problems can be solved directly by a model. Others require additional search and verification. Still others already have reliable mechanisms and are best handled through direct execution.

A system capable of sustained evolution should go further: it should change how it solves recurring classes of problems.

If a class of problems continues to depend on expensive search, learning can transfer more of that work to an AI model. If some patterns become sufficiently stable, they can be formalized as mechanisms. If the assumptions behind an existing mechanism change, models and search can be reintroduced.

Self-improvement in AI should therefore not be limited to

$$
\theta_t\rightarrow\theta_{t+1}
$$

The fuller transition is a change in the entire solution architecture:

$$
\boxed{\mathcal A_t\rightarrow\mathcal A_{t+1}}
$$

The most important AI systems of the future may not simply possess stronger models. They may continually optimize the structure of their own solutions.

---

## Conclusion

The framework can be reduced to four claims.

**First, every solution is constrained by both computational cost and uncertainty.**

**Second, search, AI models, and mechanisms are three fundamental ways of managing that tradeoff, and they work together.**

**Third, as a problem matures, the center of gravity of its solution system tends to shift from search to AI models and then to mechanisms.**

**Fourth, rightward shifts in existing problems allow finite computational resources to be invested at a larger frontier of unknown problems.**

Progress in intelligence is not only about solving harder problems. It is also about finding better ways to solve problems already encountered, so that computation can be redirected toward a larger unknown world.

The key to the next generation of AI may therefore be more than a stronger model. It may be a solution system capable of continually moving existing problems rightward while expanding its own problem frontier.
