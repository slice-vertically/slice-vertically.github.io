---
layout: post-with-tags
title:  "JSON fields in PostgreSQL - UPDATE"
date:   2024-03-16
tags:   postgres database json
---

## UPDATE

### A JSON field

id | personal_info
--- | ---
123 | {first_name: 'John', last_name: 'Falstaff'}  

```sql
UPDATE persons
SET personal_info = jsonb_set(
	personal_info::jsonb,
	'{first_name}',
	'Mary'
)
```

By PostgreSQL's documentation, `jsonb_set(target, path, new_value)` "returns *target* with the section designated by *path* replaced by *new_value*"

> `jsonb_set()` has a fourth optional boolean parameter *create_missing* which allows you to specify whether you want to add the path with the *new_value* if *path* didn't exist.

See [Postgres' documentation on JSON functions](https://www.postgresql.org/docs/current/functions-json.html).