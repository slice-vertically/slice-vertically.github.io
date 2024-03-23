---
layout: post-with-tags
title:  "Limit Oracle DB query"
date:   2024-03-23
tags:   oracle database
---

```plsql
SELECT *
FROM YOUR_TABLE
FETCH FIRST 10 ROWS ONLY;
```