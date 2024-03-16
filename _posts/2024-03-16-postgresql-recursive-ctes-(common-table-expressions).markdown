---
layout: post-with-tags
title:  "PostgreSQL Recursive CTEs (Common Table Expressions)"
date:   2024-03-16
tags:   postgres database
---

Useful when you have a tree or graph structure.
It needs a **base case** and a **recursive case**, and they have to be joined by a `UNION`.

Suppose a `users` table:
```sql
CREATE TABLE users (  
	id INT PRIMARY KEY,
	parent INT NULL,
	content varchar NULL
);
```

With the following hierarchy (an admin, two managers underneath the admin, users underneath the managers):

```sql
INSERT INTO users VALUES (1, NULL, 'admin');  
INSERT INTO users VALUES (2, 1, 'manager1');  
INSERT INTO users VALUES (3, 1, 'manager2');  
INSERT INTO users VALUES (4, 2, 'user1');  
INSERT INTO users VALUES (5, 2, 'user2');  
INSERT INTO users VALUES (6, 3, 'user3');  
INSERT INTO users VALUES (7, 3, 'user4');  
INSERT INTO users VALUES (8, 3, 'user5');
```

In order to retrieve a user and all of their children you could write a recursive CTE like this:

```sql
WITH RECURSIVE hierarchy(_id, _parent, _content) AS (
	-- base case
	SELECT id, parent, content FROM users WHERE id = 2
	
	UNION
	
	-- recursive case
	SELECT id, parent, content
	FROM hierarchy, users
	WHERE parent = _id
)
SELECT * FROM hierarchy;
```

| _id | _parent | _content |
| ---- | ---- | ---- |
| 2 | 1 | manager1 |
| 4 | 2 | user1 |
| 5 | 2 | user2 |
