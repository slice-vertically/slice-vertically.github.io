---
layout: post-with-tags
title:  "Get duplicates for specific values Oracle DB query"
date:   2024-03-23
tags:   oracle database
---

```sql
GROUP BY column_with_duplicated_value_1, column_with_duplicated_value_2
HAVING COUNT(*) >= 2
```

## Example

```sql
CREATE TABLE emp (
	empno number(4) constraint pk_emp primary key,
	ename varchar2(10),
	hiredate date, 
	deptno number(2)
);

INSERT ALL
INTO emp VALUES (7369,'SMITH',to_date('17-12-1980','dd-mm-yyyy'),20)
INTO emp VALUES (7521,'WARD',to_date('22-2-1981','dd-mm-yyyy'),30)
INTO emp VALUES (7566,'JONES',to_date('2-4-1981','dd-mm-yyyy'),20)
INTO emp VALUES (7654,'MARTIN',to_date('28-9-1981','dd-mm-yyyy'),30)
INTO emp VALUES (7698,'BLAKE',to_date('1-5-1981','dd-mm-yyyy'),30)
INTO emp VALUES (7782,'CLARK',to_date('9-6-1981','dd-mm-yyyy'),10)
INTO emp VALUES (7839,'KING',to_date('17-11-1981','dd-mm-yyyy'),10)
INTO emp VALUES (7876,'ADAMS',to_date('13-07-1987','dd-mm-yyyy'),20)
INTO emp VALUES (7900,'JAMES',to_date('3-12-1981','dd-mm-yyyy'),30)
INTO emp VALUES (7902,'FORD',to_date('3-12-1981','dd-mm-yyyy'),20)
INTO emp VALUES (7934,'MILLER',to_date('13-07-1987','dd-mm-yyyy'),20)
SELECT 1 FROM DUAL; -- hack for versions smaller than Oracle 23c
```

```sql
SELECT deptno, hiredate
FROM emp
GROUP BY deptno, hiredate
HAVING COUNT(*) >= 2;
```

| deptno | hiredate |
| ---- | ---- |
| 20 | 13-JUL-87 |


> ℹ Docker images for Oracle Database are not publicly accessible so to test this I used [this online tool](https://dbfiddle.uk).
