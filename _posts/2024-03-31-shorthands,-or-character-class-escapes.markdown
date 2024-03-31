---
layout: post-with-tags
title:  "Shorthands, or character class escapes"
date:   2024-03-31
tags:   regexp
---

> ℹ Examples shown in JS so that they're easy to check.

## \\w and \\W

```javascript
"123abc_-*&#".match(/\w/g);
// ['1', '2', '3', 'a', 'b', 'c', '_']
```
```javascript
"123abc_-*&#".match(/\W/g);
// ['-', '*', '&', '#']
```

## \\d and \\D

```javascript
"123abc_-*&#".match(/\d/g);
// ['1', '2', '3']
```
```javascript
"123abc_-*&#".match(/\D/g);
// ['a', 'b', 'c', '_', '-', '*', '&', '#']
```
## \\s and \\S

```javascript
"the 1 ring!".match(/\s/g);
// [' ', ' ']
```
```javascript
"the 1 ring!".match(/\S/g);
// ['t', 'h', 'e', '1', 'r', 'i', 'n', 'g', '!']
```