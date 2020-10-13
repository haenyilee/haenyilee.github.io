---
sort: 8
---

# JOIN

## JOIN이란?
- 하나의 SQL 명령문에 의해 디스크의 여러개의 테이블에서 사용자가 요청한 데이터를 메모리로 복사해서 가지고 와 조회도 하고 변경도 할 수 있는 기능
- 메모리는 작업을 하는 공간, 디스크는 저장을 하는 공간

## JOIN의 종류

![](https://lh3.googleusercontent.com/proxy/p7pFiPxj-mSeqTzV6oZg34u3D1oh6zofg5CV9VZ9Lz_0F7xWFNzc0QJ-3-3tfONaw3UDt_U4Lc2zuBav-xoW7KcI19sTeDDGZnRsp8z2WoXRqwkea7gXcoIZgwkiwQ6SIS66fwkDckqkfhHW6hGhFOzrr7luJzGg43rh8j_LoHb2a7QaUIXeldPnJLMMlGX2qjdRzwDC4MX7Mb7bZTHom6T0zdJ0_93MWPEfX6E8t_6_SVgPrLzrFzsZUi3B4TI6rIUanSTf7kPufuH2xOIA2POUUVHtGb-_PVKhyaqnueyB67_ED3W-HXE5_rQEHDAglN597BY)

  - 카티션곱
  - INNER JOIN
  - OUTER JOIN
  - SELF JOIN

## Cartesian Product(카티션 곱)

```oracle
-- INNER JOIN
SELECT empno,ename,job,mgr,hiredate,sal,comm,e.deptno,dname,loc 
FROM emp e,dept d WHERE e.deptno=d.deptno 
ORDER BY ename;

-- 카디션 곱
SELECT empno,ename,job,mgr,hiredate,sal,comm,e.deptno,dname,loc 
FROM emp e,dept d 
ORDER BY ename;
```


## INNER JOIN 
- 교집합
- 다른 테이블에서 데이터를 가져와서 연결
- NULL값일때 처리가 불가능
  - NULL값이 있는 경우에는 처리하지 않는다.
  - NULL값 처리를 위해서 기능을 확장한 것이 OUTER JOIN
- 종류 : EQUI Join(등가 조인) , Non-Equi Join(비등가 조인)

### ```EQUI_JOIN``` : 등가조인
- 가장 많이 사용되는 기술
- 연산자 ```=``` : 같은 값일때 가져오기
- (예시) EMP테이블의 deptno와 DEPT테이블의 deptno가 같을 때, DEPT테이블의 loc, dept컬럼을 가져와라
  - DEPT테이블의 deptno는 ```Primary Key (기본키)```
  - EMP테이블의 deptno는 ```Foreign Key (참조키) ```


- 등가조인의 형식


<html>
<table>
<tr>
<th colspan=4>EQUI_JOIN의 형식</th>
</tr>
<tr>
<th>오라클조인</th><th>ANSI 조인</th><th>NATURAL 조인</th><th>JOIN-USING</th>
</tr>
<tr>
<td colspan=2 align=center>컬럼명이 다를 수 있다</td><td colspan=2 align=center>반드시 같은 컬럼명이 존재해야함</td>
</tr>
<tr>
<td></td><td colspan=2 align=center>컬럼명이 같을 때 사용 권장</td><td></td>
</tr>
<tr>
<td align=center>오라클에서만 사용</td>
<td align=center>전체데이터베이스 <br> 표준화 형식</td>
<td align=center>오라클에서만 사용</td>
<td align=center>오라클에서만 사용</td>
</tr>
<tr>
<td>SELECT 컬럼명,컬럼명...<br>
FROM 테이블명,테이블명...<br>
WHERE 테이블명.col=테이블명.col<br>
(컬럼명이 다른 경우에는 <br> <i>테이블명(별칭). <i> 생략 가능)</td>
<td>
SELECT 컬럼명, 컬럼명..<br>
FROM 테이블명 <br>
(INNER) JOIN 테이블명<br>
ON 테이블명.col=테이블명.col
</td>
<td>
SELECT 컬럼명1,컬럼명2....<br>
FROM 테이블명A <br>
NATURAL JOIN 테이블명B;
</td>
<td>
SELECT 컬럼명1,컬럼명2....<br>
FROM 테이블명A <br>
JOIN 테이블명B USING(deptno);
</td>
</tr>
</table>
</html>


- (참고) 테이블 별칭 
  - 테이블명이 길 때 사용
  - as 안붙여줘도 됨

```oracle
table : emp,dept
SELECT 컬럼명....
FROM emp e, dept d;
```

#### ```오라클 조인``` : 컬럼명이 다를 수도 있다 , 오라클에서만 사용하는 쿼리문장

```
SELECT empno,ename,job,mgr,hiredate,sal,comm,e.deptno,dname,loc FROM emp e,dept d WHERE e.deptno=d.deptno ORDER BY ename;
```


```oracle
SELECT empno,ename,job,mgr,hiredate,sal,comm,deptno,dname,loc FROM emp e,dept d
                                             *
ERROR at line 1:
ORA-00918: column ambiguously defined
```

```
SELECT empno,ename,job,mgr,hiredate,sal,comm,deptno,dname,loc FROM emp e,dept d WHERE e.deptno=d.deptno
                                             *
ERROR at line 1:
ORA-00918: column ambiguously defined
```



```
-- 이름이 SCOTT인 사원의 이름, 입사일, 부서명, 근무지 출력
SELECT ename, hiredate, dname, loc 
FROM emp, dept
WHERE emp.deptno=dept.deptno
AND ename='SCOTT';
```




#### ```ANSI 조인```  : 컬럼명이 다를 수도 있다 , 전체 데이터베이스에서 사용하는 쿼리
  - JOIN 2번 걸기 : 오라클조인 형식처럼 ```AND```사용 불가 , JOIN~ON을 여러번 사용해줘야함


```
SELECT empno,ename,job,mgr,hiredate,sal,comm,e.deptno,dname,loc FROM emp e JOIN dept d ON e.deptno=d.deptno;
```


```
-- 이름이 SCOTT인 사원의 이름, 입사일, 부서명, 근무지 출력
SELECT ename, hiredate, dname, loc 
FROM emp
JOIN dept 
ON emp.deptno=dept.deptno 
WHERE emp.ename='SCOTT';
```




#### ```NATURAL 조인``` : 반드시 같은 컬럼명이 존재해야함
  - 별칭을 안줘도 됨

```
SELECT empno,ename,job,mgr,hiredate,sal,comm,deptno,dname,loc FROM emp NATURAL JOIN dept;
```




#### ```JOIN-USING``` : 반드시 같은 컬럼명이 존재해야함

```
SELECT empno,ename,job,mgr,hiredate,sal,comm,deptno,dname,loc 
FROM emp JOIN dept USING(deptno);
```



### ```NON_EQUI_JOIN```
- EQUI_JOIN과의 차이점
  - EQUI_JOIN은 ```=``` (등위연산자)
  - NATURAL-JOIN과 JOIN~USING은 사용하지 못한다(컬럼명이 무조건 같아야하니까...?)
  - 연산자를 사용하되, ```=```이 아닌 연산자를 주로 사용한다
  - 비교연산자 , BETWEEN~AND 연산자를 주로 사용한다.

```
-- emp 테이블의 sal컬럼이 salgrade테이블의 losal과 hisal사이에 있을때 join시켜라?
SELECT empno,ename,job,mgr,hiredate,sal,comm,deptno,grade 
FROM emp,salgrade 
WHERE sal BETWEEN losal AND hisal;
```


```
// ORACLE-JOIN
-- JOIN 2번 걸기(emp + dept + salgrade)
SELECT empno,ename,job,mgr,hiredate,sal,comm,emp.deptno,dname, loc,grade     // deptno에 별칭 꼭 작성해줘야함
FROM emp,dept, salgrade 
WHERE emp.deptno=dept.deptno  
AND sal BETWEEN losal AND hisal;  // 조건을 여러개 걸때는 AND로 연결
```



```
// ANSI-JOIN
-- JOIN 2번 걸기(emp + dept + salgrade)
SELECT empno,ename,job,mgr,hiredate,sal,comm,emp.deptno,dname, loc,grade 
FROM emp 
JOIN dept
ON emp.deptno=dept.deptno 
JOIN salgrade 
ON sal BETWEEN losal AND hisal;
```



## OUTER JOIN
- UNION과 유사, 합집합
- 다른 테이블에서 데이터를 가져와서 연결
- INNER JOIN값 + @ (NULL)
- NULL값일때도 처리할 수 있음
- (예시) 이름, 부서위치를 출력하는데 BOSTON도 출력되도록 하시오
  - 없는 쪽에 (+)붙이면 null값도 출력됨
  - emp.deptno에 없는데 dept.deptno에 있는 값을 가져와야하니까 Right에 (+)붙여주면 됨

```
-- Right OUTER JOIN
SELECT ename,loc FROM emp,dept WHERE emp.deptno(+)=dept.deptno;
```

### OUTER JOIN의 종류 (NULL의 위치의 반대?)
#### 1. LEFT OUTER JOIN : INTERSECT + MINUS
  - 오라클조인

```
SELECT A.col , B.col
FROM A , B
WHERE A.col=B.col(+)   // INNER JOIN +(A-B)
```

  - ANSI조인

```
SELECT A.col , B.col
JOIN A LEFT OUTER JOIN B
ON A.col=B.col   // A-B
```



#### 2. RIGHT OUTER JOIN : INTERSECT + MINUS
  - 오라클조인

```
SELECT A.col , B.col
FROM A , B
WHERE A.col(+)=B.col   // INNER JOIN+(B-A)
```

  - ANSI조인

```
SELECT A.col , B.col
JOIN A RIGHT OUTER JOIN B
ON A.col=B.col   // A-B
```

#### 3. FULL OUTER JOIN : UNION
  - ANSI조인

```
SELECT A.col , B.col
JOIN A FULL OUTER JOIN B
ON A.col=B.col   // A-B
```

## INNER JOIN과 OUTER JOIN 연습

```
SQL> select * from A;

        NO TEXT
---------- --------------------
         1 aaa
         2 bbb
         3 ccc
         4 ddd

SQL> select * from B;

        NO TEXT
---------- --------------------
         2 eee
         3 fff
         4 ggg
         5 hhh
```

- INNER JOIN : (A ∩ B) (2, 3, 4)

```
SELECT A.no,A.text,B.no,B.text
FROM A,B
WHERE A.no=B.no;
```

- LEFT OUTER JOIN : (A-B) (1) + (A ∩ B) (2, 3, 4)

```
SELECT A.no,A.text,B.no,B.text
FROM A,B
WHERE A.no=B.no(+);
```



- RIGHT OUTER JOIN : (A ∩ B) (2, 3, 4) + (B-A) (5)  

```
SELECT A.no,A.text,B.no,B.text
FROM A,B
WHERE A.no(+)=B.no;
```

- FULL OUTER JOIN : (A ∪ B) (1,2,3,4,5)

```
SELECT A.no,A.text,B.no,B.text
FROM A FULL OUTER JOIN B
ON A.no=B.no;
```

- B집합이 A집합의 부분집합일때 OUTER JOIN의 결과는?

```
SELECT ename, job,hiredate,sal,dname,loc 
FROM emp,dept 
WHERE emp.deptno=dept.deptno;

SELECT ename, job,hiredate,sal,dname,loc 
FROM emp,dept 
WHERE emp.deptno=dept.deptno(+);

// 결과값 같음
```


## SELF JOIN
- 같은 테이블에서 데이터 연결
- NULL값은 출력하지 않음

```
-- emp 테이블 내에서 사수가 같은 사람들의 이름을 출력하시오
```

|empno|mgr|ename|
|-----|---|-----|
|7788|7566|SCOTT|

|empno|mgr|ename|
|-----|---|-----|
|7566|7839|JONES|


```
// null값이 있는 것은 검색하지 못함

SELECT e1.ename "본인" , e2.ename "사수" 
FROM emp e1,emp e2    // 같은 테이블 사용할 경우 별칭 사용 필수!
WHERE e1.mgr=e2.empno;
```

```
// null값까지 검색하고 싶을 때 => OUTER JOIN 사용해야함

-- LEFT OUTER JOIN
SELECT e1.ename "본인" , e2.ename "사수" 
FROM emp e1,emp e2 
WHERE e1.mgr=e2.empno(+);

-- RIGHT OUTER JOIN
SELECT e1.ename "본인" , e2.ename "사수" 
FROM emp e1,emp e2 
WHERE e1.mgr(+)=e2.empno;

-- FULL OUTER JOIN
SELECT e1.ename "본인" , e2.ename "사수" 
FROM emp e1 FULL OUTER JOIN emp e2 
ON e1.mgr=e2.empno;
```

