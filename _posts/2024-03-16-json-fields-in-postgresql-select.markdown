---
layout: post-with-tags
title:  "JSON fields in PostgreSQL - SELECT"
date:   2024-03-16
tags:   postgres database json
---

## SELECT

### By JSON field

id | personal_info
--- | ---
123 | {first_name: 'John', last_name: 'Falstaff'}

```sql
SELECT *
FROM persons
WHERE personal_info->>'last_name' = 'Falstaff'
```

### By JSON array entry

id | personal_info
--- | ---
123 | {first_name: 'John', last_name: 'Falstaff', phones: [602951571, 658805579]}

```sql
SELECT *
FROM persons
WHERE personal_info->phones->0 = '602951571'
```

> The simple arrow (**->**) allows **treating the data as JSON** (accessing fields or positions of an array) while the double arrow (**->>**) **casts the data to text** (allowing, for example, comparing to a string).

See [Postgres' documentation on JSON functions](https://www.postgresql.org/docs/current/functions-json.html).
