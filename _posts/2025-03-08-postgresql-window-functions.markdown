---
layout: post-with-tags
title:  "PostgreSQL window functions"
date:   2025-03-08
tags:   postgres database
---

```sql
WINDOW_FUNCTION(column_name) OVER (PARTITION BY column_name ORDER BY column_name)
```

## Example

```sql
SELECT depname, empno, salary, RANK() OVER (PARTITION BY depname ORDER BY salary DESC)
FROM empsalary;
```

See [this article](https://www.postgresql.org/docs/current/tutorial-window.html).