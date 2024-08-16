---
layout: post-with-tags
title:  "Binomial coefficient"
date:   2024-04-28
tags:   statistics
---

Also called _n choose k_, it counts the number of ways to choose _k_ elements from a set of _n_ elements.

<p><code style="padding: 8px 12px 8px 12px;">
<sup>n!</sup>/<sub>k!(n - k)!</sub>
</code></p>

We'll express it as `C(n, k)`.


## Example

You have a set of 4 colours and you need to choose 3. How many distinct possibilities are there?

<div style="overflow-x: auto;">
	<table><tr>
		<td>Red</td>
		<td>Green</td>
		<td>Blue</td>
		<td>Yellow</td>
	</tr></table>
</div>

Color 1 | Color 2 | Color 3
--- | --- | ---
Red | Green | Blue
Red | Green | Yellow
Red | Blue | Yellow
Green | Blue | Yellow

<p><code style="padding: 8px 12px 8px 12px;">
<sup>n!</sup>/<sub>k!(n - k)!</sub>
</code></p>

<p><code style="padding: 8px 12px 8px 12px;">
<sup>4!</sup>/<sub>3! (4 - 3)!</sub>
</code></p>
<p><code style="padding: 8px 12px 8px 12px;">
<sup>4!</sup>/<sub>3! (1)!</sub>
</code></p>
<p><code style="padding: 8px 12px 8px 12px;">
<sup>4!</sup>/<sub>3!</sub>
</code></p>
<p><code style="padding: 8px 12px 8px 12px;">
<sup>24</sup>/<sub>6</sub> = <strong>4</strong>
</code></p>
