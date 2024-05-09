---
layout: post-with-tags
title:  "Bernouilli distribution"
date:   2024-05-09
tags:   statistics
---

The Bernouilli distribution is a special case of the [binomial distribution](/2024/05/09/binomial-distribution.html) where:

**n = 1**
```
C(1, k) · p^k (1 - p)^1-k
```

and where either:
- **k = 0**
```
C(1, 0) · p^0 (1 - p)^1-0
1 · 1 (1 - p)^1
(1 - p)
```

- **k = 1**
```
C(1, 1) · p^1 (1 - p)^1-1
1 · p (1 - p)^0
1 · p · 1
p
```

## Example

A coin toss can either come up heads (`p`) or tails (`1 - p`). If it's fair, `p=0.5`, so it will be a 50% chance of either side.
