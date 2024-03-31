---
layout: post-with-tags
title:  "Positive and negative lookahead and lookbehind"
date:   2024-03-31
tags:   regexp
---

> ℹ Examples shown in JS so that they're easy to check.

## ?= (positive lookahead)

```js
"superman".match(/super(?=man)/);
// ['super']
"superdog".match(/super(?=man)/);
// null
```

## ?! (negative lookahead)


```js
"superman".match(/super(?!man)/);
// null
"superdog".match(/super(?!man)/);
// ['super']
```

## ?<= (positive lookbehind)

```js
"superman".match(/(?<=super)man/);
// ['man']
"spiderman".match(/(?<=super)man/);
// null
```

## ?<! (negative lookbehind)

```js
"superman".match(/(?<!super)man/);
// null
"spiderman".match(/(?<!super)man/);
// ['man']
```