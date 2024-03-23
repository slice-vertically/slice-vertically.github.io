---
layout: post-with-tags
title:  "Get duplicates for specific values Oracle DB query"
date:   2024-03-23
tags:   oracle database
---

```plsql
GROUP BY column_with_duplicated_value_1, column_with_duplicated_value_2
HAVING COUNT(*) >= 2
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
INTO emp VALUES (7876,'ADAMS','CLERK',to_date('13-07-1987', 'dd-mm-yyyy'),20)  
INTO emp VALUES (7900,'JAMES','CLERK',to_date('3-12-1981','dd-mm-yyyy'),30)  
INTO emp VALUES (7902,'FORD','ANALYST',to_date('3-12-1981','dd-mm-yyyy'),20)  
INTO emp VALUES (7934,'MILLER','CLERK',to_date('13-07-1987','dd-mm-yyyy'),10)  
SELECT 1 FROM DUAL; -- hack for versions samller than Oracle 23c
```

```plsql
SELECT job, hiredate  
FROM emp
GROUP BY job, hiredate  
HAVING COUNT(*) >= 2;
```

| job | hiredate |
| ---- | ---- |
| CLERK | 1987-07-13 |
