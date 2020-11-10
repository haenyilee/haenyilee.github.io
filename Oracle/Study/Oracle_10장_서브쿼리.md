---
sort: 15
---

# 서브쿼리
- 책 : 443page

## Sub Query가 무엇일까요?



## Sub Query의 종류

### 단일행 서브쿼리
- 서브쿼리의 결과값이 1개
- 단일행 서브쿼리의 연산자 : 비교연산자(= , !=(<>) , <= , >= , < , >)

```oracle
SELECT * FROM emp WHERE empno=7369;
```

- 형식

```oracle
-- 메인쿼리
SELECT * FROM emp
-- 서브쿼리
WHERE deptno=(SELECT ~)
```

- 예) emp테이블 : 사원의 평균 급여 = 평균보다 적게 받는 사원 정보를 출력

```oracle
SELECT * FROM emp
WHERE sal<(SELECT AVG(sal) FROM emp);
```

- 예) SCOTT이 근무하는 부서에 같이 근무하는 사원의 모든 정보를 출력

```oracle
SELECT * FROM emp
WHERE deptno=(SELECT deptno FROM emp WHERE ename='SCOTT');
```

- GROUP BY를 사용한 서브쿼리
- 예) 사원의 평균 급여보다 높은 부서의 부서번호, 인원수를 출력하기

```oracle
SELECT deptno,COUNT(*)
FROM emp
GROUP BY deptno
HAVING AVG(sal)<(SELECT AVG(sal) FROM emp);
```



### 다중행 서브쿼리
- 서브쿼리 데이터 결과가 여러개인 방법이다.
- 데이터 전체를 처리하는 방법 : IN
- 최대값을 가져오는 방법 : ALL , MAX()
- 최소값을 가져오는 방법 : ANY , MIN() , SOME()

- `>ANY()` : 수행된 결과 중에 최소값  , `>ANY(SELECT deptno FROM dept) ANY (10,20,30,40,50) => 10`
- `<ANY()` : 수행된 결과 중에 최대값 , `>ANY(SELECT deptno FROM dept) ANY (10,20,30,40,50) => 50`
- `>ALL()` : 수행된 결과 중에 최대값 , `>ANY(SELECT deptno FROM dept) ALL (10,20,30,40,50) => 50`
- `<ALL()` : 수행된 결과 중에 최소값 , `>ANY(SELECT deptno FROM dept) ALL (10,20,30,40,50) => 10`


- ANY , SOME, ALL

|종류|역할|
|----|---|
|ANY,SOME|메인 쿼리의 비교 조건이 서브 쿼리의 검색 결과와 하나 이상 일치하면 값을 반환|
|ALL|메인 쿼리의 비교 조건이 서브 쿼리의 검색 결과와 모든 값이 일치하면 값을 반환|


- 수행된 결과 중에 최소값 가져오기

```oracle
SELECT empno,ename,job,deptno
FROM emp
WHERE deptno>ANY(SELECT DISTINCT deptno FROM emp);
```

- 중복되지 않는 값 가져오기

```oracle
SELECT empno,ename,job,deptno
FROM emp
WHERE deptno IN (SELECT DISTINCT deptno FROM emp);
```



### 인라인 뷰 , TOP-N(rownum)
- FROM(SELECT~)
- SELCT는 테이블과 컬럼을 대신할 수 있다.

- rownum은 중간의 데이터를 짤라올 수 없다는 단점이 있다.

```oracle
-- 출력불가
SELECT ename,job,sal,rownum FROM emp
WHERE rownum BETWEEN 5 AND 10;
```

- 인기순위, 베스트 댓글

```oracle
SELECT ename,job,sal,rownum FROM emp
WHERE rownum<=5;
```

- 예) 급여가 많은 사람 5명

```ORACLE
SELECT ename,job,sal,rownum 
FROM (SELECT ename,job,sal FROM emp ORDER BY sal DESC)
WHERE rownum<=5
ORDER BY sal DESC;
```

- 예) 급여가 5~10위인 사람 5명

```oracle
SELECT ename,job,sal,num
FROM (SELECT ename,job,sal, rownum as num
FROM (SELECT ename,job,sal FROM emp ORDER BY sal DESC))
WHERE num BETWEEN 5 AND 10;
```

### Scalar Sub Query(스칼라 서브 쿼리)
- 컬럼 대신에 사용하는 방법
- 데이터가 많으면 많을수록 join을 걸면 속도가 느려지기 때문에 스칼라 서브쿼리를 많이 이용한다.

```oracle
SELECT empno,ename,(SELECT ~), (SELECT ~) as dname
```

- 조인

```oracle
SELECT empno,ename,job,dname,loc
FROM emp,dept
WHERE emp.deptno=dept.deptno;
```

- 조인을 스칼라 서브쿼리로 대체하기

```orale
SELECT empno,ename,job,
(SELECT dname FROM dept d WHERE d.deptno=e.deptno) as dname,
(SELECT loc FROM dept d WHERE d.deptno=e.deptno) as loc 
FROM emp e;
```

- 예) 사원 정보 : 사번, 이름 , 직위, 입사일 , 부서명 , 근무지 , 급여등급 / KING과 같은 부서의 사람들의 정보를 출력하기

```oracle
SELECT empno,ename,job,hiredate,
(SELECT dname FROM dept d WHERE d.deptno=e.deptno) as dname, 
(SELECT loc FROM dept d WHERE d.deptno=e.deptno) as loc, 
(SELECT grade FROM salgrade WHERE sal BETWEEN losal AND hisal) as grade
FROM emp e
WHERE deptno=(SELECT deptno FROM emp WHERE ename='KING');
```
