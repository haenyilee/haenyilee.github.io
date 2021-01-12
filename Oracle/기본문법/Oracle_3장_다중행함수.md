---
sort: 3
---

# [Oracle] 다중행 함수

## 집합함수
- 세로 COLUMN으로만 계산함(다중행함수)
- 가로는 직접 연산 처리해야함(단일행함수)
- 컬럼이나 단일행 함수를 같이 사용하면 안된다.
  - 컬럼마다 출력되는 데이터의 갯수가 상이하면 테이블을 생성할 수 없기 때문에 칼럼마다 출력되는 데이터 갯수가 일치해야 한다.
- GROUP BY를 사용하면 가능하다.

```
-- emp 전체 통계(최대급여,최소급여,사원수)를 구하시오
SELECT MAX(sal) "최대급여", MIN(sal) "최소급여", COUNT(*) "사원수", 
SUM(sal) "급여 총합", AVG(sal) "급여평균" FROM emp;
```



## GROUP함수의 종류

#### ```COUNT``` : 갯수
- 로그인, ID 중복 체크에 주로 쓰임
- COUNT(*) : NULL값을 포함
- COUNT(컬럼명) : NULL값을 제외
- 그룹함수끼리만 써야함(```SELECT COUNT(*),ename FROM emp;```처럼 작성하면 오류)


```
SQL> SELECT COUNT(*), COUNT(mgr),COUNT(comm) FROM emp;

  COUNT(*) COUNT(MGR) COUNT(COMM)
---------- ---------- -----------
        14         13           3
```

```
-- 이름이 king인 row 수를 구하여라?
select COUNT(*) FROM emp WHERE ename='king';
```

```
-- genie_music의 페이지 나누는 쿼리문장을 작성하시오
SELECT COUNT(*) FROM genie_music;
SELECT CEIL(COUNT(*)/10.0) FROM genie_music;
```

#### ```MAX``` : 최대값 , ```MIN``` : 최소값
- 주어진 데이터 중에서 가장 큰 값, 가장 작은값을 돌려줌
- 날짜에도 사용 가능함
- 최소, 최대값 구할때는 주로 인덱스 사용함
- NULL값을 무시(제외)

- 자동 증가, 자동 감소 번호 매길때 주로 사용함

```
-- 신규사원에게 중복되지 않을 사번을 부여하라
SELECT MAX(empno)+1 FROM emp;
INSERT INTO emp(empno,ename) VALUES((SELECT MAX(empno)+1 FROM emp),'HongGD1');
INSERT INTO emp(empno,ename) VALUES((SELECT MAX(empno)+1 FROM emp),'HongGD2');
INSERT INTO emp(empno,ename) VALUES((SELECT MAX(empno)+1 FROM emp),'HongGD3');
INSERT INTO emp(empno,ename) VALUES((SELECT MAX(empno)+1 FROM emp),'HongGD4');
INSERT INTO emp(empno,ename) VALUES((SELECT MAX(empno)+1 FROM emp),'HongGD5');
```


```
-- 신규사원 등록하기(사번:중복x,입사일:금일,이름:Hong)
INSERT INTO emp(empno,ename,hiredate,deptno) VALUES((SELECT MAX(empno)+1 FROM emp),'Hong',SYSDATE,10);
```




#### ```SUM``` : 합계 , ```AVG``` : 평균값
- NULL값을 무시(제외) 
- AVG 구할 때는 ROUND로 소수점 정리해주기
- SUM 구할 때는 TO_STRING으로 천의 자리 구분해주기

```
-- 급여의 평균과 총합계를 구하여라
SQL> SELECT AVG(sal),SUM(sal) FROM emp;
```

#### ```STDDEV``` : 표준편차, ```VARIANCE``` : 분산

```
-- 급여의 표준편차와 분산을 구하시오.
SELECT STDDEV(sal), VARIANCE(sal) FROM emp;
```


#### ```ROLLUP``` : 소계합
- 가로줄(행,ROW단위,tuple)별로 계산해서 통계내주는 함수

```
-- ROLLUP으로 부서별로 급여 통계내기
SELECT deptno,job,COUNT(*),ROUND(AVG(sal),2) FROM emp GROUP BY ROLLUP(deptno, job);
```

```
-- 교수 직책별 보너스의 합계를 구하여라
SELECT
position, subject, SUM(bonus)
FROM
PROFESSOR
GROUP BY ROLLUP(position,  subject)
```

```
-- 과목별 보너스의 합계를 구하여라
SELECT
subject, position, SUM(bonus)
FROM
PROFESSOR
GROUP BY ROLLUP(subject, position)
```



#### ```CUBE```
- 가로계산(행,ROW단위,tuple) 후 세로계산(열,COLUMN단위,attribute)을 통해 전체통계를 내줌

```
-- 직업별로 총갯수와 평균도 구하고, 부서별로 총갯수와 급여평균도 구하여라
SELECT deptno,job,COUNT(*),ROUND(AVG(sal),2) FROM emp GROUP BY CUBE(deptno, job);

```


#### ```RANK OVER(ORDER BY 컬럼명 ASC|DESC)``` 
- 랭킹 매겨주는 함수(중복카운팅O, 3위-3위-5위)
- ```,```으로 연결

```
SELECT ename,sal, RANK() OVER(ORDER BY sal DESC) "rank" FROM emp;
```

#### ```DENSE_RANK OVER(ORDER BY 컬럼명 ASC|DESC)```
- 랭킹 매겨주는 함수dd
- 중복 카운팅X(3위-3위-4위)
- ```,```으로 연결

```
SELECT ename,sal, DENSE_RANK() OVER(ORDER BY sal DESC) "rank" FROM emp;
```


```
-- 급여가 많은 TOP5를 출력하라
SQL> SELECT rownum,ename,sal FROM emp WHERE rownum<=5 ORDER BY sal DESC;

    ROWNUM ENAME                       SAL
---------- -------------------- ----------
         4 JONES                      2975
         2 ALLEN                      1600
         5 MARTIN                     1250
         3 WARD                       1250
         1 SMITH                       800
```

```
-- 급여가 많은 TOP5를 출력하라
SQL> SELECT rownum,ename,sal FROM (SELECT ename,sal FROM emp ORDER BY sal DESC) WHERE rownum<=5;

    ROWNUM ENAME                       SAL
---------- -------------------- ----------
         1 KING                       5000
         2 SCOTT                      3000
         3 FORD                       3000
         4 JONES                      2975
         5 BLAKE                      2850
```











```REGEXP_LIKE```



## GROUP BY절을 통한 세부적 그룹화
- 형식 : ```GROUP BY 컬럼명```
- 뒤에 적힌 컬럼을 기준으로 값을 먼저 모아두고 SELECT 절에 적혀 있는 그룹함수를 적용함

```
-- 포지션 별로 모아서 보너스의 합계를 구하여라
SELECT position, sum(bonus)
FROM professor
GROUP BY position;
```


```
-- 10,20,30 그룹별로 인원수, 급여합, 급여평균 출력하시오
SELECT deptno, COUNT(*), SUM(sal), AVG(sal) FROM emp GROUP BY deptno;
SELECT COUNT(*), SUM(sal), AVG(sal) FROM emp WHERE deptno=10;
SELECT COUNT(*), SUM(sal), AVG(sal) FROM emp WHERE deptno=20;
SELECT COUNT(*), SUM(sal), AVG(sal) FROM emp WHERE deptno=30;

-- [ERROR!] 싱글function이 포함되어 있음
SELECT deptno, COUNT(*), SUM(sal), AVG(sal) FROM emp WHERE deptno=10;
SELECT deptno, COUNT(*), SUM(sal), AVG(sal) FROM emp WHERE deptno=20;
SELECT deptno, COUNT(*), SUM(sal), AVG(sal) FROM emp WHERE deptno=30;
```

- 그룹핑할 조건이 여러 개라면? : GRUOP BY절에 이어서 작성하면 됨
```
SELECT deptno, job, COUNT(*) FROM emp 
GROUP BY (deptno,job)
ORDER BY deptno;
```

- SELECT절에 사용된 그룹 함수 이외의 컬럼이나 표현식은 반드시 GRUOP BY절에 사용되어야 함

```
-- job별로 모은 뒤에 mgr별로 모아서 그들끼리의 급여 최대,최소값을 구하여라
SELECT job,mgr, COUNT(*), MAX(sal), MIN(sal) FROM emp GROUP BY (job,mgr) ORDER BY job;
```

```
-- 부서별, 직무별로 급여평균(소수점 3번째 자리에서 반올림)구하기
SELECT deptno,job,COUNT(*),ROUND(AVG(sal),2) FROM emp GROUP BY deptno, job;
```


```
-- 입사년도별로  인원수, 급여합, 급여평균 출력하시오
SELECT TO_CHAR(hiredate,'YYYY'),COUNT(*),SUM(sal),AVG(sal) FROM emp GROUP BY TO_CHAR(hiredate,'YYYY');

-- [ERROR!]
SELECT COUNT(*),SUM(sal),AVG(sal) FROM emp GRUOP BY SUBSTR(hiredate,1,2);
```


## HAVING절을 통한 그룹핑 조건 검색

- WHERE절은 그룹함수의 비교 조건으로 쓸 수 없음
  - WHERE문장 뒤에는 집합함수(그룹함수)가 아닌 단일행조건만 올 수 있음
```
WHERE SUM(sal)=4000 // 사용불가(집합함수)
WHERE job!='SALESMAN' // 사용가능(단일행)
```

```
경우에 따라서는 HAVING 절을 사용하여 그룹 전체에 조건을 적용하기 전에 
WHERE 절을 사용하여 그룹에서 개별 행을 제외해야 할 수도 있습니다.
HAVING 절은 WHERE 절과 비슷하지만 그룹 전체 즉, 
그룹을 나타내는 결과 집합의 행에만 적용된다는 점에서 차이가 있습니다. 
반면, WHERE 절은 개별 행에 적용됩니다. 

쿼리에는 WHERE 절과 HAVING 절이 모두 포함될 수 있습니다. 이 경우 다음을 수행합니다.
  - 다이어그램 창에서 테이블이나 테이블 반환 개체의 개별 행에 WHERE 절이 먼저 적용됩니다. 
    WHERE 절의 조건에 맞는 행만 그룹화됩니다.

  - 그런 다음 결과 집합의 행에 HAVING 절이 적용됩니다. 
  - HAVING 조건에 맞는 그룹만 쿼리 출력에 표시됩니다. 
  - 집계 함수나 GROUP BY 절에도 나타나는 열에만 HAVING 절을 적용할 수 있습니다.
```


- 위치 : GROUP BY 절의 앞 or 뒤

```
-- 부서별로 급여의 합, 평균을 구한다 **조건: 전체 평균보다 많이 받는 부서만 출력**
SELECT deptno,COUNT(*),SUM(sal),AVG(sal) FROM emp GROUP BY deptno 
HAVING AVG(sal)>(SELECT AVG(sal) FROM emp);
```

--3 부서번호,부서번호별 최대 월급을 출력하되, 20번은 제외하고 출력하시오
SELECT deptno, MAX(sal) FROM emp WHERE deptno!=20 GROUP BY deptno ORDER BY deptno;
SELECT deptno, MAX(sal) FROM emp GROUP BY deptno HAVING deptno!=20 ORDER BY deptno;
