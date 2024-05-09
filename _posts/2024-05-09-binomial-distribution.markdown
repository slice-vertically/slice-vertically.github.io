---
layout: post-with-tags
title:  "Binomial distribution"
date:   2024-05-09
tags:   statistics
---

A binomial distribution is a function that gives the probability of successes given a number of independent trials.

```
C(n, k) · p^k (1 - p)^n-k
```

Where `C(n, k)` is the [binomial coefficient](/2024/04/28/binomial-coefficient.html).

## Example

You are running an A/B test. 20% of customers click variation A, the rest (80%) click variation B.

What's the probability of getting two clicks on variation A in the next 3 visits?

| Visit 1 | Visit 2 | Visit 3 | TOTAL |
| --- | --- | --- | --- |
| A (0.2) | A (0.2) | A (0.2) | 0.2 · 0.2 · 0.2 = 0.008 |
| **A (0.2)** | **A (0.2)** | **B (0.8)** | **0.2 · 0.2 · 0.8 = 0.032** |
| **A (0.2)** | **B (0.8)** | **A (0.2)** | **0.2 · 0.8 · 0.2 = 0.032** |
| A (0.2) | B (0.8) | B (0.8) | 0.2 · 0.8 · 0.8 = 0.128 |
| **B (0.8)** | **A (0.2)** | **A (0.2)** | **0.8 · 0.2 · 0.2 = 0.032** |
| B (0.8) | A (0.2) | B (0.8) | 0.8 · 0.2 · 0.8 = 0.128 |
| B (0.8) | B (0.8) | A (0.2) | 0.8 · 0.8 · 0.2 = 0.128 |
| B (0.8) | B (0.8) | B (0.8) | 0.8 · 0.8 · 0.8 = 0.512 |

The probabilities for "two A clicks" all work out to be 0.032, because we are expecting two A clicks (0.2) and one B (0.8). In other words: 
```
0.2 · 0.2 · 0.8 = 0.032
```
Or, using exponents
```
0.2^2 · 0.8^1 = 0.032
```
The _0.2_ is the probability the choice we want, call it _p_.

The _2_ is the number of choices we want, call it _k_.
```
p^k · 0.8^1
```
The _0.8_ is the probability of the opposite choice, so that is _1−p_.

The _1_ is the number of opposite choices, so it is _n−k_.
```
p^k · (1 - p)^n-k
```

The only thing that's missing is we need to include that there are **three** different ways this can happen, as we saw in the table above: (A, A, B) or (A, B, A) or (B, A, A).

We do that with the formula for the [binomial coefficient](/2024/04/28/binomial-coefficient.html), `C(n, k)` (in this example `C(3, 2) = 3! / 2! = 3`).
