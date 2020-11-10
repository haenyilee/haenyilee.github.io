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

|종류|의미|
|----|---|
|ANY,SOME|메인 쿼리의 비교 조건이 서브 쿼리의 검색 결과와 하나 이상 일치하면 값을 반환|
|ALL|메인 쿼리의 비교 조건이 서브 쿼리의 검색 결과와 모든 값이 일치하면 값을 반환|


### 인라인 뷰
- FROM(SELECT~)




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

- 조인을 서브쿼리로 대체하기

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
