---
layout: post-with-tags
title:  "Get first of each group Oracle DB query"
date:   2024-03-23
tags:   oracle database
---

```plsql
MIN(interesting_column) KEEP (DENSE_RANK FIRST ORDER BY interesting_column)
	OVER (PARTITION BY grouping_column) AS interesting_column_alias
```

## Example

```plsql
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
INTO emp VALUES (7876,'ADAMS',to_date('13-JUL-87', 'dd-mm-rr')-51,20)
INTO emp VALUES (7900,'JAMES',to_date('3-12-1981','dd-mm-yyyy'),30)
INTO emp VALUES (7902,'FORD',to_date('3-12-1981','dd-mm-yyyy'),20)
INTO emp VALUES (7934,'MILLER',to_date('23-1-1982','dd-mm-yyyy'),10)
SELECT 1 FROM DUAL; -- hack for versions smaller than Oracle 23c
```

Get the employee with the smallest hiredate for each department (`10`, `20` or `30`):


```plsql
SELECT
	DISTINCT deptno, -- for each department 
	MIN(hiredate) KEEP (DENSE_RANK FIRST ORDER BY hiredate)
		OVER (PARTITION BY deptno) AS first_hire, -- smallest hiredate
	MIN(ename) KEEP (DENSE_RANK FIRST ORDER BY hiredate)
		OVER (PARTITION BY deptno) AS ename-- from smallest hiredate, ename
FROM emp;
```

| deptno | first_hire | ename |
| ---- | ---- | ---- |
| 10 | 09-JUN-81 | CLARK |
| 20 | 17-DEC-80 | SMITH |
| 30 | 22-FEB-81 | WARD |

See [this article](https://oracle-base.com/articles/misc/first-and-last-analytic-functions).


> ℹ Docker images for Oracle Database are not publicly accessible so to test this I used [this online tool](https://dbfiddle.uk).