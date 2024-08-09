---
layout: post-with-tags
title:  "PostgreSQL check schema permissions"
date:   2024-03-16
tags:   postgres database
---

```sql
SELECT * FROM pg_namespace;
```
<div style="overflow-x: auto;">
	<table>
	  <thead>
	    <tr>
	      <th>oid</th>
	      <th>nspname</th>
	      <th>nspowner</th>
	      <th>nspacl</th>
	    </tr>
	  </thead>
	  <tbody>
	    <tr>
	      <td>1</td>
	      <td>test_schema</td>
	      <td>10</td>
	      <td>{user2=UC/user1}</td>
	    </tr>
	  </tbody>
	</table>
</div>

In this case, `user2` has **U**SAGE and **C**REATION permissions on the `test_schema` granted by `user1`.

See [the privileges Postgres docs](https://www.postgresql.org/docs/current/ddl-priv.html#PRIVILEGES-SUMMARY-TABLE).