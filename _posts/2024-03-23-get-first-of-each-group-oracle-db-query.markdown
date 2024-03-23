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
	job varchar2(9),
	hiredate date, 
	deptno number(2)
);
  
INSERT ALL  
INTO emp VALUES (7369,'SMITH','CLERK',to_date('17-12-1980','dd-mm-yyyy'),20)  
INTO emp VALUES (7521,'WARD','SALESMAN',to_date('22-2-1981','dd-mm-yyyy'),30)  
INTO emp VALUES (7566,'JONES','MANAGER',to_date('2-4-1981','dd-mm-yyyy'),20)  
INTO emp VALUES (7654,'MARTIN','SALESMAN',to_date('28-9-1981','dd-mm-yyyy'),30)  
INTO emp VALUES (7698,'BLAKE','MANAGER',to_date('1-5-1981','dd-mm-yyyy'),30)  
INTO emp VALUES (7782,'CLARK','MANAGER',to_date('9-6-1981','dd-mm-yyyy'),10)  
INTO emp VALUES (7839,'KING','PRESIDENT',to_date('17-11-1981','dd-mm-yyyy'),10)  
INTO emp VALUES (7876,'ADAMS','CLERK',to_date('13-JUL-87', 'dd-mm-rr')-51,20)  
INTO emp VALUES (7900,'JAMES','CLERK',to_date('3-12-1981','dd-mm-yyyy'),30)  
INTO emp VALUES (7902,'FORD','ANALYST',to_date('3-12-1981','dd-mm-yyyy'),20)  
INTO emp VALUES (7934,'MILLER','CLERK',to_date('23-1-1982','dd-mm-yyyy'),10)  
SELECT 1 FROM DUAL; -- hack for versions samller than Oracle 23c
```

Get the employee with the smallest hiredate for each job (`CLERK`, `MANAGER` or `SALESMAN`):


```plsql
SELECT  
	DISTINCT deptno, -- for each department 
	MIN(hiredate) KEEP (DENSE_RANK FIRST ORDER BY hiredate)
		OVER (PARTITION BY deptno) AS first_hire, -- smallest hiredate
	MIN(ename) KEEP (DENSE_RANK FIRST ORDER BY hiredate)
		OVER (PARTITION BY deptno) AS ename  -- from smallest hiredate, ename
FROM emp;
```

| empno | ename | job | hiredate | deptno |
| ---- | ---- | ---- | ---- | ---- |
| 7782 | CLARK | MANAGER | 1981-06-09 | 10 |
| 7369 | SMITH | CLERK | 1980-12-17 | 20 |
| 7521 | WARD | SALESMAN | 1981-02-22 | 30 |

See [this article](https://oracle-base.com/articles/misc/first-and-last-analytic-functions).
