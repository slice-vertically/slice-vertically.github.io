---
layout: post-with-tags
title:  "Crontab expressions"
date:   2024-11-16
tags:   bash
---

```
min hour day(m) month day(w)
```

## Examples

Every Monday at 6:00AM:

```
0 6 * * 1
```

Every minute on the 3rd and 5th of each month:

```
* * 3,5 * *
```

Every weekday at 9:30AM:

```
30 9 * * 1-5
```
