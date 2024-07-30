---
layout: post-with-tags
title:  "Bernouilli distribution"
date:   2024-05-09
tags:   statistics
---

The Bernouilli distribution is a special case of the [binomial distribution](/2024/05/09/binomial-distribution.html) where:

**n = 1**
<p><code style="padding: 8px 12px 8px 12px;">
C(1, k) × p<sup>k</sup> (1 - p)<sup>1-k</sup>
</code></p>

and where either:
- **k = 0**
<p><code style="padding: 8px 12px 8px 12px;">
C(1, 0) × p<sup>0</sup> (1 - p)<sup>1-0</sup>
</code></p>

<p><code style="padding: 8px 12px 8px 12px;">
1 × 1 (1 - p)<sup>1</sup>
</code></p>

<p><code style="padding: 8px 12px 8px 12px;">
<strong>(1 - p)</strong>
</code></p>

- **k = 1**
<p><code style="padding: 8px 12px 8px 12px;">
C(1, 1) × p<sup>1</sup> (1 - p)<sup>1-1</sup>
</code></p>
<p><code style="padding: 8px 12px 8px 12px;">
1 × p (1 - p)<sup>0</sup>
</code></p>
<p><code style="padding: 8px 12px 8px 12px;">
1 × p × 1
</code></p>
<p><code style="padding: 8px 12px 8px 12px;">
<strong>p</strong>
</code></p>

<br>

## Example

A coin toss can either come up heads (`p`) or tails (`1 - p`). If it's fair, `p=0.5`, so it will be a 50% chance of either side.
