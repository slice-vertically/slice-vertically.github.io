---
layout: post-with-tags
title:  "Binomial distribution"
date:   2024-05-09
tags:   statistics
---

A binomial distribution is a function that gives the probability of successes given a number of independent trials.

<p><code style="padding: 8px 12px 8px 12px;">
C(n, k) × p<sup>k</sup> (1 - p)<sup>n-k</sup>
</code></p>


Where `C(n, k)` is the [binomial coefficient](/2024/04/28/binomial-coefficient.html).

## Example

You are running an A/B test. 20% of customers click variation A, the rest (80%) click variation B.

What's the probability of getting two clicks on variation A in the next 3 visits?

<div style="overflow-x: auto;">
	<table>
	  <thead>
	    <tr>
	      <th>Visit 1</th>
	      <th>Visit 2</th>
	      <th>Visit 3</th>
	      <th>TOTAL</th>
	    </tr>
	  </thead>
	  <tbody>
	    <tr>
	      <td>A (0.2)</td>
	      <td>A (0.2)</td>
	      <td>A (0.2)</td>
	      <td>0.2 × 0.2 × 0.2 = 0.008</td>
	    </tr>
	    <tr>
	      <td><strong>A (0.2)</strong></td>
	      <td><strong>A (0.2)</strong></td>
	      <td><strong>B (0.8)</strong></td>
	      <td><strong>0.2 × 0.2 × 0.8 = 0.032</strong></td>
	    </tr>
	    <tr>
	      <td><strong>A (0.2)</strong></td>
	      <td><strong>B (0.8)</strong></td>
	      <td><strong>A (0.2)</strong></td>
	      <td><strong>0.2 × 0.8 × 0.2 = 0.032</strong></td>
	    </tr>
	    <tr>
	      <td>A (0.2)</td>
	      <td>B (0.8)</td>
	      <td>B (0.8)</td>
	      <td>0.2 × 0.8 × 0.8 = 0.128</td>
	    </tr>
	    <tr>
	      <td><strong>B (0.8)</strong></td>
	      <td><strong>A (0.2)</strong></td>
	      <td><strong>A (0.2)</strong></td>
	      <td><strong>0.8 × 0.2 × 0.2 = 0.032</strong></td>
	    </tr>
	    <tr>
	      <td>B (0.8)</td>
	      <td>A (0.2)</td>
	      <td>B (0.8)</td>
	      <td>0.8 × 0.2 × 0.8 = 0.128</td>
	    </tr>
	    <tr>
	      <td>B (0.8)</td>
	      <td>B (0.8)</td>
	      <td>A (0.2)</td>
	      <td>0.8 × 0.8 × 0.2 = 0.128</td>
	    </tr>
	    <tr>
	      <td>B (0.8)</td>
	      <td>B (0.8)</td>
	      <td>B (0.8)</td>
	      <td>0.8 × 0.8 × 0.8 = 0.512</td>
	    </tr>
	  </tbody>
	</table>
</div>

The probabilities for "two A clicks" all work out to be 0.032, because we are expecting two A clicks (0.2) and one B (0.8). In other words: 

<p><code style="padding: 8px 12px 8px 12px;">
0.2 × 0.2 × 0.8 = 0.032
</code></p>

Or, using exponents
<p><code style="padding: 8px 12px 8px 12px;">
0.2<sup>2</sup> × 0.8<sup>1</sup> = 0.032
</code></p>

The _0.2_ is the probability the choice we want, call it _p_.

The _2_ is the number of choices we want, call it _k_.
<p><code style="padding: 8px 12px 8px 12px;">
p<sup>k</sup> × 0.8<sup>1</sup>
</code></p>
The _0.8_ is the probability of the opposite choice, so that is _1−p_.

The _1_ is the number of opposite choices, so it is _n−k_.
<p><code style="padding: 8px 12px 8px 12px;">
p<sup>k</sup> × (1 - p)<sup>n-k</sup>
</code></p>

The only thing that's missing is we need to include that there are **three** different ways this can happen, as we saw in the table above: (A, A, B) or (A, B, A) or (B, A, A).

We do that with the formula for the [binomial coefficient](/2024/04/28/binomial-coefficient.html), `C(n, k)` (in this example <code style="padding: 8px 12px 8px 12px;">C(3, 2) = <sup>3!</sup>/<sub>2! = 3</sub></code>).
