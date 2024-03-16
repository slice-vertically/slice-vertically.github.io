---
layout: post-with-tags
title:  "PostgreSQL roles and permissions"
date:   2024-03-16
tags:   postgres database
---

## Creation and assigning

You can create a role:

```sql
CREATE ROLE role1;
```

And then assign it:

```sql
GRANT role1 to user1;
GRANT role1 to user2;
```

And now all the permissions of `role1` will be available to `user1` and `user2`:

```sql
GRANT SELECT ON table1 TO role1;
```

```sql
-- user1
SELECT * FROM table1; -- OK

-- user2
SELECT * FROM table1; -- OK
```

## Inheritance

Permissions can also be inherited:

```sql
CREATE ROLE role2;

GRANT SELECT ON table2 TO role2;

GRANT role2 TO role1;
```


```sql
-- user1
SELECT * FROM table2; -- OK

-- user2
SELECT * FROM table2; -- OK
```
