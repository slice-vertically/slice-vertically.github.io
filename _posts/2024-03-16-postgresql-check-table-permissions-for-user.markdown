---
layout: post-with-tags
title:  "PostgreSQL check table permissions for user"
date:   2024-03-16
tags:   postgres database
---

## Tables a user owns

```sql
SELECT *
FROM pg_tables
WHERE tableowner = 'USER_NAME';
```

## Tables a user has permissions for

```sql
SELECT *
FROM information_schema.role_table_grants
WHERE grantee = 'USER_NAME';
```