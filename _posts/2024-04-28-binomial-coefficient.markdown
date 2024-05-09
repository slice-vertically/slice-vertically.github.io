---
layout: post-with-tags
title:  "Binomial coefficient"
date:   2024-04-28
tags:   statistics
---

Also called _n choose k_, it counts the number of ways to choose _k_ elements from a set of _n_ elements.

```
n! / k! (n - k)!
```

We'll express it as `C(n, k)`.


## Example

You have a set of 4 colours and you need to choose 3. How many distinct possibilities are there?

Red | Green | Blue | Yellow

Color 1 | Color 2 | Color 3
--- | --- | ---
Red | Green | Blue
Red | Green | Yellow
Red | Blue | Yellow
Green | Blue | Yellow

```
n! / k! (n - k)!
```

```
4! / 3! (4 - 3)!
4! / 3! (1)!
4! / 3!
24 / 6 = 4
```
