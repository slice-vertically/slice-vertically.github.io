---
layout: post-with-tags
title:  "Human readable date from UNIX timestamp using bash"
date:   2024-11-16
tags:   bash
---

```sh
date -d @1961916400000
```

```sh
# portable version
perl -le 'print scalar localtime $ARGV[0]' 1961916400000
```
