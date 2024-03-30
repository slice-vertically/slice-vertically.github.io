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


> ℹ Docker images for Oracle Database are not publicly accessible so to test this I used [this online tool](https://dbfiddle.uk).