## 목차
1. [단일행함수 VS 복수행함수](#단일행함수-vs-복수행함수)
2. [단일행함수](#단일행함수)
3. [문자 함수](#문자-함수)
LENGTH, 
SUBSTR, 
RPAD, 
INSTR
1) [변환함수](#변환함수)
UPPER, LOWER, INITCAP, REPLACE

2) [제어함수](#제어함수)
CONCAT, SUBSTR, INSTR

3) [기타함수](#기타함수)
LENGTH, LENGTHB, LPAD, RPLAD, TRIM, RTRIM, LTRIM


4. [숫자함수](#숫자함수)
ROUND, TRUNC, 
CEIL, MOD

5. [날짜함수](#날짜함수)
SYSDATE, 
MONTH_BETWEEN

6. [변환함수](#변환함수)
TO_CHAR



7. [기타함수](#기타함수)
NVL, 

[]()
8. [정규식(Regular Expression) 함수](#정규식(Regular Expression)-함수)




## 단일행함수 VS 복수행함수
- 단일행함수 : 한줄씩(ROW) 바꿔나가는 함수
  - 예) 한줄씩 검색해서 NULL값을 없애라(변환함수)

![](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F23133E34550799AE17)

- 복수행함수 : 전체 ROW를(열)를 대상으로 데이터를 가져오는 함수
  - 예) 부서번호가 10번은 사람들을 찾아라 => 부서번호 열 전체를 검색해야함

# 단일행함수

![](https://lh3.googleusercontent.com/proxy/k3q9SNd9nbMY29J3SMyqbqbr3hKmiHz_SqxWkjvUJbj9Lzz23lJuvAevoJeqoIZzVvitigqCNeXvLTVIAQKYSnHdZf01A4Ksr8lKtiD2cReGiWw)

# 문자 함수

![](https://kookyungmin.github.io/image/Oracle_image/Oracle_image_72.png)

## 변환함수
```
SQL> SELECT ename,UPPER(ename),LOWER(ename),INITCAP(ename) FROM emp;

ENAME                UPPER(ENAME)         LOWER(ENAME)         INITCAP(ENAME)
-------------------- -------------------- -------------------- --------------------
SMITH                SMITH                smith                Smith
```

- ```UPPER``` : 대문자로 변환시켜주는 함수

- ```LOWER``` : 소문자로 변환시켜주는 함수

- ```INITCAP``` : 첫글자만 대문자로 바꿔줌

- ```REPLACE``` : 문자를 변경
  - 형식 : REPLACE ('문자열' , '찾는문자' , '변경문자')
```
-- 이름에 A가 나오면 M으로 변경하기
SQL> SELECT ename,REPLACE(ename,'A','M') FROM emp;

```


## 제어함수

- ```CONCAT``` : 문자열 결합 = ||


```
SQL> SELECT CONCAT('Hello','Oracle'),'Hello'||'Oracle' FROM DUAL;

CONCAT('HELLO','ORACLE)   'HELLO'||'ORACLE'
----------------------    ----------------------
HelloOracle               HelloOracle
```

- ```SUBSTR``` : 문자를 분해 ≒ subString()
  - 형식 : SUBSTR _( 문자열 , 시작번호 , 자를글자수 )_
  - 문자번호가 1번부터 시작함을 유의해야함
  - 두번째 글자가 '~까지'가 아닌 자를 글자의 갯수를 의미함
  - 시작번호가 (-)로 시작하면 뒤에서부터 자릿수 계산하여 문자 추출함

```
-- 1의 자리부터 3글자 자르시오
SQL> SELECT SUBSTR('HELLO ORACLE',1,3) FROM DUAL;

HEL
```


```
-- emp테이블에서 12월 입사한 사원의 모든 정보 출력
SELECT * FROM emp WHERE SUBSTR(hiredate,4,2)>=12;
```

    - 날짜 자릿수 셀때 ```/```도 포함해야함
      - 예) 81/12/03 에서 일자만 추출하려면 7번째글자(0)부터 2글자 출력해야함

```
-- 3일에 입사한 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE SUBSTR(hiredate,7,2)=03;

     EMPNO ENAME                JOB                       MGR HIREDATE        SAL       COMM     DEPTNO
---------- -------------------- ------------------ ---------- -------- ---------- ---------- ----------
      7900 JAMES                CLERK                    7698 81/12/03        950                    30
      7902 FORD                 ANALYST                  7566 81/12/03       3000                    20
```



```
-- 이름중에 3번째 자리가 A인 사원의 모든 정보 출력
방법1) SQL> SELECT * FROM emp WHERE SUBSTR(ename,3,1)='A'; // 1은 생략 가능
방법2) SQL> SELECT * FROM emp WHERE  ename LIKE '__A%';

     EMPNO ENAME                JOB                       MGR HIREDATE        SAL       COMM     DEPTNO
---------- -------------------- ------------------ ---------- -------- ---------- ---------- ----------
      7698 BLAKE                MANAGER                  7839 81/05/01       2850                    30
      7782 CLARK                MANAGER                  7839 81/06/09       2450                    10
      7876 ADAMS                CLERK                    7788 83/01/12       1100                    20
```


- ```INSTR``` : 특정 문자위치 찾기 
  - Java의 indexOf()와 유사함
  - 형식 : INSTR _( ' 찾아질문자 ' , ' 찾을문자 ' , 시작위치 , 찾을 문자의 순서 )_
    - 찾을 문자의 순서가 첫번째일경우(첫번째 나오는 '찾을문자'의 자리수를 출력하고 싶다면), 1은 생략 가능
  - 출력내용 : 찾은 문자의 자릿수

```
-- 첫번째글자부터 검색해서 3번째로 나오는 슬러시(/)의 자릿수를 구해라
SQL> SELECT INSTR('A/B/C/D/E/F','/',1,3) FROM DUAL;

INSTR('A/B/C/D/E/F','/',1,3)
----------------------------
                           6
````




## 기타함수

- ```LENGTH``` : 문자의 갯수 카운트

```
SQL> SELECT LENGTH('HONG'),LENGTH('홍길동') FROM DUAL;

LENGTH('HONG') LENGTH('홍길동')
-------------- ----------------
             4                3
```

```
-- 이름 글자수가 5자리인 사람들 출력
SQL> SELECT ename FROM emp WHERE LENGTH(ename)=5;

ENAME
--------------------
SMITH
ALLEN
JONES
BLAKE
CLARK
SCOTT
ADAMS
JAMES
```


- ```LENGTHB``` : Byte 갯수 카운트
  - 한글은 한글자를 3Byte로 인식
  - 자주 쓰이지 않음


```
SQL> SELECT LENGTHB('HONG'),LENGTHB('홍길동') FROM DUAL;

LENGTHB('HONG') LENGTHB('홍길동')
--------------- -----------------
              4                 9
```




- ```LPAD``` : 왼쪽부터 글자를 대체

```
SQL> SELECT LPAD('SCOTT',10,'*') FROM DUAL;

LPAD('SCOTT',10,'*')
--------------------
*****SCOTT
```

- ```RPAD``` : 오른쪽부터 글자를 대체

```
-- 이름 앞 두글자만 제외하고 *로 표시하기
SQL> SELECT ename, RPAD(SUBSTR(ename,1,2),LENGTH(ename),'*') FROM emp;
```

- LPAD와 RPAD의 특징
  - 지정한 글자수보다 모자라면, 부족한만큼 대체어로 메꿔주어야함(왼쪽 OR 오른쪽에서부터)
  - 만약 출력하고자 하는 글자수가 원글자수보다 짧으면 넘어서는 자릿수의 글자들은 삭제됨

```
SQL> SELECT RPAD('HELLO ORACLE',5,'#') FROM DUAL;

RPAD('HELL
----------
HELLO

SQL> SELECT RPAD('HELLO ORACLE',20,'#') FROM DUAL;

RPAD('HELLOORACLE',20,'#')
----------------------------------------
HELLO ORACLE########
```



- TRIM, RTRIM, LTRIM의 특징
  - 지정한 문자를 제거할 수 있다.
  - 형식 : LTRIM('문자열' , '지정한문자')
  - ```TRIM``` : 양쪽 시작부터 다른 문자나오기 전까지 지정한 문자를 제거
```
- 양쪽의 B삭제하기
방법(1) SQL> SELECT TRIM('B' FROM'BBBaaAAB') FROM DUAL;
방법(2) SQL> SELECT RTRIM(LTRIM('BBBaaAAB','B'),'B')FROM DUAL;
```

  - ```RTRIM``` : 오른쪽 시작부터 다른 문자나오기 전까지 지정한 문자를 제거
```
- 맨 오른쪽이 B로 시작하는 경우 오른쪽 A삭제하기
SQL> SELECT RTRIM('aaaaaaaaAAAAABAAAABBB','A') FROM DUAL;

RTRIM('AAAAAAAAAAAAABAAAABBB','A')
------------------------------------------
aaaaaaaaAAAAABAAAABBB
```

  - ```LTRIM``` : 왼쪽 시작부터 다른 문자나오기 전까지 지정한 문자를 제거

```
-- 맨왼쪽부터 다른글자가 나오기 전까지 A를 모두 제거하기
SQL> SELECT LTRIM('ABAAAA','A') FROM DUAL;

LTRIM('ABA
----------
BAAAA


-- 맨 왼쪽에 공백이 있는 경우
SQL> SELECT LTRIM(' ABAAAA','A') FROM DUAL;

LTRIM('ABAAAA'
--------------
 ABAAAA

-- 공백을 제거하고 처리하기
SELECT LTRIM(LTRIM(' ABAAAA'),'A') FROM DUAL;


-- 이름중에 왼쪽 A를 모두 제거해서 출력
SQL> SELECT ename "원본" , LTRIM(ename,'A')FROM emp;

원본                 LTRIM(ENAME,'A')
-------------------- --------------------
ALLEN                LLEN
ADAMS                DAMS
```

# 숫자함수
- ```ROUND(실수,자리수)``` : 반올림함수
  - ROUND(실수,n)이면 소수점 이하 (n+1)번째 숫자를 보고 올림할지, 내림할지 결정해주기
    - (예) ```ROUND(23.3354, 2)``` : 소수점 이하 두자리 자르기 , 소수점 3자리에서 반올림
    - (예) ```ROUND(23.3354, 0)``` : 소수점 이하 자르기, 소수점 첫번째 자리에서 반올림
    - (예) ```ROUND(23.3354,-1)``` : 일의 자리에서 반올림

```
SQL> SELECT ROUND(23.3354, 2),ROUND(23.3354, 0),ROUND(23.3354,-1) FROM DUAL;

ROUND(23.3354,2) ROUND(23.3354,0) ROUND(23.3354,-1)
---------------- ---------------- -----------------
           23.34               23                20
```


- ```TRUNC(실수,자리수)``` : 버림함수
  - (예) TRUNC(12.7859,3) : 소수점 4자리에서 버림
  - (예) TRUNC(987.654,2) : 소수점 3자리에서 버림
  - (예) TRUNC(987.654,0) : 소수점 1자리에서 버림
  - 퇴직연금, 급여계산할때 자주 쓰임

```
SQL> SELECT TRUNC(12.7859,3),TRUNC(987.654,2),TRUNC(987.654,0) FROM DUAL;

TRUNC(12.7859,3) TRUNC(987.654,2) TRUNC(987.654,0)
---------------- ---------------- ----------------
          12.785           987.65              987
```



- ```CEIL(실수)``` : 올림함수
  - (예) CEIL(12.34) = 13
```
SQL> SELECT CEIL(12.7859),CEIL(987.654),CEIL(987.65445) FROM DUAL;

CEIL(12.7859) CEIL(987.654) CEIL(987.05445)
------------- ------------- ---------------
           13           988             988
```
  - 자리수 필요없음 : (오류) CEIL(12.34,0) , CEIL(12.34,1)
  - 소수점 이하 첫번째 자리가 ```0```이면 오류


  - 총 페이지수 구할때 자주 쓰임
```
SELECT CEIL(COUNT(*)/10.0) FROM emp;
> 2 : 10개씩 나눴을 때 총 2페이지

SELECT CEIL(COUNT(*)/7.0) FROM genie_music;
> 29 : 7개씩 나눴을 때 총 29페이지

```


- ```MOD(정수,정수)``` : 나머지구하기 (Java:```%```)
```
-- 10을 3으로 나누기
SELECT MOD(10,3) FROM DUAL;
> 1 : Java에선 10%3으로 구함

-- emp에서 사번이 짝수인 사원의 사번과 이름을 출력하시오
SELECT empno,ename FROM emp WHERE MOD(empno,2)=0 ORDER BY empno;
```

# 날짜함수
- ```SYSDATE``` : 시스템의 날짜, 시간
  - 숫자로 출력되기 때문에 어제, 오늘, 내일날짜 출력이 가능함
```
-- 시스템의 오늘 날짜 읽기
SELECT SYSDATE FROM DUAL;
> 20/08/07

-- 어제 , 내일, 모레 날짜 구하기
SELECT SYSDATE-1 "어제",SYSDATE+1 "내일",SYSDATE+2 as "모레" FROM DUAL;

어제     내일     모레
-------- -------- --------
20/08/06 20/08/08 20/08/09
```

- ```MONTHS_BETWEEN``` : 기간의 개월수
```
양식 : SELECT ename,MONTHS_BETWEEN(최신날짜,이전날짜)
```


SELECT ename,ROUND(MONTHS_BETWEEN(SYSDATE,hiredate),0),ROUND(MONTHS_BETWEEN(SYSDATE,hiredate),0)/12
FROM emp;

SELECT ename,ROUND(MONTHS_BETWEEN(SYSDATE,hiredate),0),ROUND(MONTHS_BETWEEN(SYSDATE,hiredate),0)/12
FROM emp;


SELECT ename,ROUND(MONTHS_BETWEEN(SYSDATE,hiredate),0) "근속기간",TRUNC(ROUND(MONTHS_BETWEEN(SYSDATE,hiredate),0)/12)) "연차" FROM emp;


- ```ADD_MONTHS(기준일,N)``` : 개월추가
  - 기준일로부터 N개월 후의 날짜를 구해줌
```
-- 지금부터 6개월 후의 날짜 구하기
SELECT ADD_MONTHS(SYSDATE,6) FROM DUAL;
> 21/02/07

-- 학원 들어온날부터 6개월 후
SQL> SELECT ADD_MONTHS('20/06/15',6) FROM DUAL;
> 20/12/15
```

- ```NEXT_DAY(날짜,'요일')``` : 요일에 해당되는 날짜를 출력
  - 입력한 날짜 이후 돌아오는 '요일'에 해당하는 날짜를 출력

```
-- SYSDATE(20/08/07) 이후에 돌아오는 금요일은 몇년/몇월/몇일?
SELECT NEXT_DAY(SYSDATE,'금') FROM DUAL;
> 20/08/14
```



- ```LAST_DAY(YY/MM/DD)``` : 입력된 달의 마지막날을 출력

```
-- 20/08월의 마지막 날짜 출력
SELECT LAST_DAY(SYSDATE) FROM DUAL;
> 20/08/31
```

# 변환함수
- ```TO_CHAR(원래 날짜,'원하는 패턴')``` : (1) 날짜를 문자열로 변환
1. 년 : YYYY , YY , RRRR , RR , YEAR
```
SELECT SYSDATE, TO_CHAR(SYSDATE,'YYYY') "YYYY",TO_CHAR(SYSDATE,'RRRR') "RRRR",TO_CHAR(SYSDATE,'YY') "YY",TO_CHAR(SYSDATE,'RR') "RR",TO_CHAR(SYSDATE,'YEAR') "YEAR" FROM DUAL;

SYSDATE  YYYY     RRRR     YY   RR   YEAR
-------- -------- -------- ---- ---- -----------------
20/08/07 2020     2020     20   20   TWENTY TWENTY
```

2. 월 : MM , MON , MONTH
```
SELECT SYSDATE,TO_CHAR(SYSDATE,'MM') "MM", TO_CHAR(SYSDATE,'MON') "MON", TO_CHAR(SYSDATE,'MONTH') "MONTH" FROM DUAL;

SYSDATE  MM   MON              MONTH
-------- ---- ---------------- ----------------
20/08/07 08   8월              8월
```

3. 일 : DAY , DD , DDTH
```
SELECT SYSDATE,TO_CHAR(SYSDATE,'DAY') "DAY", TO_CHAR(SYSDATE,'DD') "DD", TO_CHAR(SYSDATE,'DDTH') "DDTH" FROM DUAL;

SYSDATE  DAY                      DD   DDTH
-------- ------------------------ ---- --------
20/08/07 금요일                   07   07TH

```

4. 시간 : HH ,  HH24
```
SELECT SYSDATE,TO_CHAR(SYSDATE,'HH') "HH", TO_CHAR(SYSDATE,'HH24') "HH24" FROM DUAL;

SYSDATE  HH   HH24
-------- ---- ----
20/08/07 03   15
```


5. 분 : MI
```
SELECT SYSDATE,TO_CHAR(SYSDATE,'MI') "MI" FROM DUAL;

SYSDATE  MI
-------- ----
20/08/07 22
```

6. 초 : SS
```
-- 시스템의 현재시간 구하기
SELECT SYSDATE,TO_CHAR(SYSDATE,'SS') "SS" FROM DUAL;

SYSDATE  SS
-------- ----
20/08/07 57
```

```
SELECT SYSDATE, TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') FROM DUAL;
SYSDATE  TO_CHAR(SYSDATE,'YYYY-MM-DDHH24:MI:SS'
-------- --------------------------------------
20/08/07 2020-08-07 15:25:12

-- 입사년월일을 쓰고 입력한 입사년월일 시간을 출력해보기
SELECT TO_CHAR(hiredate, 'YYYY-MM-DD HH24:MI:SS') FROM emp;
```


- ```TO_CHAR(정수,패턴형식)``` : (2) 정수를 패턴있는 문자열로 변환

|패턴|용도|예시|
|---|-----|---|
|9|9의 개수만큼 자릿수 채움|TO_CHAR(1234,'9,999,999')|
|,|천단위 구분기호를 표시|TO_CHAR(1234,'9,999,999')|
|$|달러|TO_CHAR(1234,'$9,999,999')|
|L|원|TO_CHAR(1234,'$9,999,999')|

```
SELECT TO_CHAR(1234,'9,999,999') FROM DUAL;
SELECT TO_CHAR(1234,'$9,999,999') FROM DUAL;
SELECT ename, sal, TO_CHAR(sal,'$9,999,999') FROM emp;
SELECT ename, sal, TO_CHAR(sal,'L9,999,999') FROM emp;
```


- ```TO_NUMBER('문자열','바꿀형식')``` : 정수로 변환
  - 정수로 바꾸려면, 뒤에 ```'9,999'``` 작성하면 됨
```
SQL> SELECT TO_NUMBER('5,000','9,999') FROM DUAL;

TO_NUMBER('5,000','9,999')
--------------------------
                      5000

SQL> SELECT TO_NUMBER('300','9,999') FROM DUAL;

TO_NUMBER('300','9,999')
------------------------
                     300
```



- ```TO_DATE``` : 날짜로 변환

# 일반함수
- ```NVL(공백칼럼,대체어)``` : NULL값을 다른 값으로 변경
- 대체어로 공백( ```''``` ) 주면 null값으로 인식해 오류발생
- 공백으로 대체하려면 꼭 ```' '```으로 작성해야함
```
SELECT zipcode,sido,gugun,dong,NVL(bunji,' ') FROM zipcode;
```

```
SELECT ename,sal, NVL(comm,0), sal+NVL(comm,0) FROM emp;
```

- ```DECODE(컬럼명,값1,출력값1,값2,출력값2...)``` : 다중 IF문
  - 자바에서 switch문과 유사함
```
-- 부서번호마다 부서명 매겨주기
SQL> SELECT ename, deptno, DECODE(deptno,10,'영업부',20,'개발부',30,'기획부') as dname FROM emp;

ENAME                    DEPTNO DNAME
-------------------- ---------- ------------------
SMITH                        20 개발부
ALLEN                        30 기획부
```

```
-- 지니차트 상태값 변경하기
SELECT title, DECODE(state,'유지','-','상승','▲','하강','▼') as "상태" FROM genie_music;
```

- ```CASE``` : 선택문
  - 형식 : ```CASE WHEN 바꿀값 THEN 변경값 ..... END```
```
SELECT ename,CASE WHEN deptno=10 THEN '영업부' WHEN deptno=20 THEN '개발부' WHEN deptno=30 THEN '기획부' END "dname" FROM emp;

ENAME                dname
-------------------- ------------------
SMITH                개발부
ALLEN                기획부
```


```
-- 지니차트 상태값 변경하기
SELECT title, CASE WHEN state='유지' THEN '-' WHEN state='상승' THEN '▲' WHEN state='하강' THEN '▼' END "상태" FROM genie_music;
```




# 정규식(Regular Expression) 함수
- ```RANK() OVER(ORDER BY 컬럼명)```
```
-- 월급 순위 매기기(내림차순, 중복순위 건너뛰기)
SQL> SELECT ename,sal,RANK() OVER(ORDER BY sal DESC) as rank FROM emp;

ENAME                       SAL       RANK
-------------------- ---------- ----------
KING                       5000          1
FORD                       3000          2
SCOTT                      3000          2


-- 공동 4위가 2명이라면, 다음 순위를 6번으로 매기기(올림차순)
SELECT ename,sal,RANK() OVER(ORDER BY sal) as rank FROM emp;

ENAME                       SAL       RANK
-------------------- ---------- ----------
SMITH                       800          1
JAMES                       950          2
ADAMS                      1100          3
WARD                       1250          4
MARTIN                     1250          4
MILLER                     1300          6
```

- ```DENSE_RANK() OVER(ORDER BY 컬럼명)```
```
-- 공동 4위가 2명이라면, 다음 순위를 5번으로 매기기(올림차순)
SELECT ename,sal,DENSE_RANK() OVER(ORDER BY sal) as rank FROM emp;

SQL> SELECT ename,sal,DENSE_RANK() OVER(ORDER BY sal) as rank FROM emp;

ENAME                       SAL       RANK
-------------------- ---------- ----------
SMITH                       800          1
JAMES                       950          2
ADAMS                      1100          3
WARD                       1250          4
MARTIN                     1250          4
MILLER                     1300          5
```

```
-- 몇 분기에 입사했는지 출력하기
SELECT ename,SUBSTR(hiredate,4,2) "입사월",
CASE WHEN SUBSTR(hiredate,4,2) BETWEEN '01' AND '03' THEN '1/4'
WHEN SUBSTR(hiredate,4,2) BETWEEN '04' AND '06' THEN '2/4'
WHEN SUBSTR(hiredate,4,2) BETWEEN '07' AND '09' THEN '3/4'
WHEN SUBSTR(hiredate,4,2) BETWEEN '10' AND '12' THEN '4/4'
END "분기" 
FROM emp;
```


```
-- sal 1~1000(Level 1), 1001~2000(Level 2), 2001~3000(Level 3), 3001~4000(Level 4), 4001~5000(Level 5)
(1) CASE문으로 만들기
SELECT sal, CASE WHEN sal>=1 AND sal<=1000 THEN 'LV.1'
WHEN sal>=1001 AND sal<=2000 THEN 'LV.2'
WHEN sal>=2001 AND sal<=3000 THEN 'LV.3'
WHEN sal>=3001 AND sal<=4000 THEN 'LV.4'
WHEN sal>=4001 AND sal<=5000 THEN 'LV.5'
END "연봉레벨"
FROM emp;

(2) 
SELECT sal, CASE WHEN sal BETWEEN 1 AND 1000 THEN 'LV.1'
WHEN sal BETWEEN 1001 AND 2000 THEN 'LV.2'
WHEN sal BETWEEN 2001 AND 3000 THEN 'LV.3'
WHEN sal BETWEEN 3001 AND 4000 THEN 'LV.4'
WHEN sal BETWEEN 4001 AND 5000 THEN 'LV.5'
END "연봉레벨"
FROM emp;


```


# 11g에서 추가된 정규식 함수
정규식 함수 : regexp_like


||||
|-|--|--|
|^|^A|A%|
|$|A$|%A|
|.|임의의 문자|_A__|
|*|여러문자, 0문자|[A-Z]* : 대문자 알파벳이 여러개 존재 OR 없을 수 있음|
|+|1글자 이상||
|[]|해당문자|[AB]  [A-Z]  [a-z]  [0-9] [가-힇]|
|[^]|[^A]|A를 제외|

```
-- 소문자 알파벳을 포함된 라인을 가져오라
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[a-z]');
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[[:lower:]] ');


-- 소문자 알파벳으로 시작하고 공백을 포함하는 라인을 찾아라
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[a-z] ');


-- 대문자가 포함된 라인을 찾아와라
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[[:upper]] ');

-- 한글이 포함된 라인을 가져오라
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[가-힇]');


-- 영어가 포함된 라인을 가져오라
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[A-Za-z]');

-- 소문자 알파벳으로 시작하고 + 공백 2칸 + 숫자로 끝나는 라인을 찾아라
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[a-z]  [0-9]');


-- 공백이 있는 라인을 찾아라
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[[:space:]]');

-- 대문자가 연속적으로 N글자 이상 오는 라인을 찾아라
SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[A-Z]{N}');
SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[A-Z]{3}');
SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[A-Z]{4}');


-- 알파벳으로 끝나는 모든 라인을 출력하라
SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[A-Za-z]$');


-- MA 또는 MO로 시작되는 이름을 가진 사원을 찾아라
SELECT * FROM emp WHERE REGEXP_LIKE(ename,'^M(A|O)');


-- *를 포함하는 모든 라인을 찾아라(?나 *는 앞에 구분 기호가 필요함)
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'\*');
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'\?');


-- [ - ]로 시작하지 않는 모든 데이터를 출력
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'^[^a-z]');
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'^[^A-Z]');
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'^[^0-9]');
SQL> SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'^[^가-힇]');

-- ????
SELECT * FROM regTable WHERE REGEXP_LIKE(tag,'[^a-z]');

```

