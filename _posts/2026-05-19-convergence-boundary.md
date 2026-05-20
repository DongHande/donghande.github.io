---
layout: post
title: "Why Scaling Compute Can't Save LLMs"
date: 2026-05-19
description: "A structural analysis of why LLMs fail at non-convergent tasks — from first principles."
---

> My day job is training large language models and building Agent applications. This post comes from a structural problem I keep running into in practice.

---

A large model with a million-token context window can digest an entire book in seconds. But ask it to execute a real-world task that requires three months of iteration in a changing environment, and it falls apart.

It's not unintelligent. It just doesn't converge.

This post explains why — from first principles — and what it means for how humans and AI divide labor.

---

## I. Two Paths

Both LLMs and human brains learn by extracting patterns from experience. The difference isn't who's smarter. It's their preference for model complexity.

**The silicon path: maximize model complexity.**

The core strategy of Transformer architectures is statistical fitting — the more parameters, the higher the precision in fitting the training distribution. Scaling Laws tell us: more compute, more data, more fine-grained patterns captured. The philosophy: encode every detail of the current environment as precisely as possible.

**The carbon path: minimize model complexity.**

The human brain runs on 20 watts. Working memory handles roughly seven items at once. This hardware constraint forced a completely different strategy: don't encode every detail. Actively discard most information. Keep only a handful of rules with the fewest possible variables. These rules are low-precision — rough, lossy, missing enormous amounts of detail. But simple models have a property that complex models cannot achieve: they don't depend on any specific time, place, or environmental condition. They hold across time, across space, across contexts.

An analogy: precise fitting is a tailored suit — perfect fit, but only for one occasion. Rough compression is a loose shell jacket — unflattering, but works in wind, rain, or mountains.

One sentence:

**AI pursues precise but fragile fitting. Humans pursue rough but invariant compression.**

This isn't a value judgment. It's a difference in which strategy suits which type of problem. And this difference determines what each can and cannot solve.

---

## II. The Cards Each Side Holds

Given the path difference above, the specific capabilities of each side follow naturally.

### Silicon's three cards

These are natural derivatives of "precise fitting + brute-force compute":

**Long context.** It remembers what you've forgotten. It can surface faint correlations buried in hundreds of thousands of words of discarded information.

**High bandwidth.** Reports you can't finish in a day, it ingests in a second. Its information reach is virtually unlimited.

**Tireless execution.** You make careless mistakes after eight hours of work. It runs a month-long task chain with the same error rate on day one and day thirty. It consumes no willpower. It doesn't fatigue.

### Carbon's three cards

These are evolutionary necessities forced by the 20-watt hardware constraint:

**Filtering.** Knowing what doesn't matter is more important than knowing what does. The human brain comes with a powerful noise filter — it lets us cut straight to the trunk without processing everything.

**Energy efficiency.** One meal's worth of calories powers a full day of complex decision-making. We can make directional judgments while consuming almost no external resources.

**Invariance modeling.** This is the most critical card. Humans tend to build models that are simple to the point of seeming crude — "supply and demand determine price," "people act irrationally when afraid." These models lose massive amounts of detail, but precisely because they're simple enough, they don't depend on any specific time, place, or environmental condition. True ten years ago, true now, probably true in a different country.

### A common misconception

"AI is more rational because it has no emotions." — Human risk aversion isn't an irrational bug. It's the optimal survival strategy computed by genes through billions of years of lethal selection — an extraordinarily cold, reality-tested optimization result.

---

## III. Convergence: The Only Boundary That Matters

With two paths and their respective cards laid out, we return to the opening question: why does the LLM perform brilliantly on some tasks and completely collapse on others?

The answer lies in one concept: **convergence**.

Whether a strategy converges depends on whether its underlying assumptions change. If your model is built on invariants — physical laws, fundamental structures of human nature — then no matter how the environment shifts, errors shrink. If your model is built on current environmental details — this quarter's market preferences, this cohort's behavioral patterns — then once the environment shifts, errors start compounding. The former converges. The latter doesn't.

The essence of convergence is **error decay**: you make a small mistake, and the system absorbs it, returning to track.
The essence of non-convergence is **error amplification**: you make a small mistake, and the system amplifies it exponentially until total loss of control — this is precisely what the butterfly effect means.

![Convergence vs Divergence](/assets/blog/convergence-boundary/convergence-diagram.svg)

### The convergent world: AI's absolute domain

A system is convergent if it satisfies these conditions:
- Closed rules — the game doesn't change midway
- Immediate feedback — you know right away whether you're right or wrong
- Stable environment — what works today still works tomorrow

In such a world, AI's precise fitting is the ultimate trump card. The environment doesn't change, so higher precision means closer to optimal. More compute, more data — errors steadily shrink, answers get closer.

A concrete example: code compilation. A decade ago, "is this logic correct?" was an open question — you might have to wait for user feedback. Today, compilers, type systems, and automated tests form a perfect immediate-feedback sandbox. Inside this sandbox, AI can try endlessly, each attempt near-zero cost, each feedback instant and precise. So AI's coding ability is converging rapidly.

**"Sandboxing" means using engineering infrastructure to forcibly convert an open problem into a convergent closed system.**

### The non-convergent world: AI's structural dead end

But most real-world scenarios don't look like this. Their characteristics:
- Extremely long feedback cycles — you make a decision and might not know if it's right for two or three years
- Continuous environment shift — today's effective strategy may be invalidated next month by policy changes or market disruption
- Small deviations compound — each misjudgment accumulates, with later deviations growing ever larger

In such a world, AI's precise fitting becomes a fatal flaw. The more precisely it fits, the more tightly it's bound to the training distribution. Once the environment shifts, those strategies precise to four decimal places instantly become worthless.

Human "rough but invariant" models, precisely because they never depended on any specific environmental details, don't break no matter how the environment changes. "Is this person fundamentally reliable?" — this judgment held ten years ago, holds now, and probably holds in a different industry.

Another concrete example: investing in a person or an early-stage company. One real feedback signal in three years. Market environment shifts countless times in between. AI's precise fitting — statistical patterns based on historical data — shatters at the first distribution shift. What survives those three years are the few minimal, rough, environment-independent judgment rules in the investor's head.

### The hard ceiling of LLMs

Now we can precisely answer the opening question:

Every step of an LLM's reasoning carries small statistical errors. In convergent systems, these errors get corrected by immediate environmental feedback — hence brilliant performance. In non-convergent systems, nothing corrects these errors. They compound step by step until the output completely departs from reality.

This isn't an engineering problem of "not strong enough yet." It's the mathematical fate of statistical fitting when facing error-amplifying systems. More parameters, longer context, more training data — none of this changes the fact. Because the problem isn't insufficient precision. The problem is that precision itself is the source of fragility.

---

## IV. The Tipping Point of Division of Labor

If you accept the derivation in Part III, the logic of human-AI division of labor becomes clear.

### The game structure

**Human intervention = using invariance models to predict direction.** Upside: prevents AI from recklessly executing in non-convergent environments, burning resources exponentially. Downside: human models are too rough, potentially pruning counterintuitive paths that could have led to unexpected success.

**AI autonomous execution = brute-force search within convergent sandboxes.** Upside: finds precise solutions that human intuition would never reach. Downside: once outside the sandbox, reckless execution losses get exponentially consumed by error amplification.

When to let humans decide, when to let AI try — depends on how costly trial-and-error is, and whether the system is converging.

Between these two modes lies a dynamic equilibrium point.

### What moves this equilibrium

The answer: sandbox infrastructure.

Every time engineering infrastructure successfully converts a previously non-convergent domain into a convergent closed system — just as compilers and automated tests transformed the code world — AI gains another safe territory, and the equilibrium shifts one step toward AI.

This leads to a judgment critical for the industry:

**The core competitive advantage of future AI products isn't bigger models or longer context. It's who can convert more real-world tasks into convergent closed environments. Building better sandboxes matters more than building bigger models.**

### Why humans won't be displaced

A final question: if sandbox infrastructure keeps advancing and AI's territory keeps growing, will humans eventually be squeezed out entirely?

No. Three reasons:

First, the world as a whole doesn't converge. Localities can be packaged into sandboxes through engineering infrastructure, but the whole cannot. Real-world rules keep mutating. New uncertainties keep emerging.

Second, undone things always outnumber done things. Once the old world is taken over by sandboxes, humans move on to open new frontiers. Human society is not a zero-sum game.

Third, as long as environments keep changing, "rough but invariant" will always be worth more than "precise but fragile." This is determined by mathematical properties — in error-amplifying systems, invariance is the only thing that survives time.

---

Carbon draws one invariant line with 20 watts. Silicon fills both sides of that line precisely with megawatts.
