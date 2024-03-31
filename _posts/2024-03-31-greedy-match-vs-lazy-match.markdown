---
layout: post-with-tags
title:  "Greedy match vs Lazy match"
date:   2024-03-31
tags:   regexp
---

> ℹ Examples shown in JS so that they're easy to check.

## \* (greedy match)

```js
"titanic".match(/t.*i/);
// titani
```
## \*? (lazy match)

```js
"titanic".match(/t.*?i/);
// ti
```