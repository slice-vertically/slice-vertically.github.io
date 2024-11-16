---
layout: post-with-tags
title:  "Base 64 encoding decoding using bash"
date:   2024-11-16
tags:   bash
---

```sh
echo -n "Hello" | base64
# "SGVsbG8="
```

```sh
echo -n "SGVsbG8=" | base64 --decode
# "Hello"
```
