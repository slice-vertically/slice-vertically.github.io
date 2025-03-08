---
layout: post-with-tags
title:  "Oracle window functions"
date:   2025-03-08
tags:   oracle database
---

```sql
WINDOW_FUNCTION(column_name) OVER (PARTITION BY column_name ORDER BY column_name)
```

## Example

```sql
SELECT
    DISTINCT deptno,
    FIRST_VALUE(hiredate) OVER (PARTITION BY deptno ORDER BY hiredate ASC) AS first_hire,
    FIRST_VALUE(ename) OVER (PARTITION BY deptno ORDER BY hiredate ASC) AS ename
FROM emp;
```

See [this article](https://oracle-base.com/articles/misc/analytic-functions).

> ℹ Docker images for Oracle Database are not publicly accessible so to test this I used [this online tool](https://dbfiddle.uk).