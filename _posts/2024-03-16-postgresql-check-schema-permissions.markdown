---
layout: post-with-tags
title:  "PostgreSQL check schema permissions"
date:   2024-03-16
tags:   postgres database
---

```sql
SELECT * FROM pg_namespace;
```

| oid | nspname | nspowner | nspacl |
| ---- | ---- | ---- | ---- |
| 1 | test_schema | 10 | {user2=UC/user1} |

In this case, `user2` has **U**SAGE and **C**REATION permissions on the `test_schema` granted by `user1`.

See [the privileges Postgres docs](https://www.postgresql.org/docs/current/ddl-priv.html#PRIVILEGES-SUMMARY-TABLE).