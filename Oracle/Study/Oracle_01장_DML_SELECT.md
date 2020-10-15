---
sort: 5
---

# SQL언어

![](https://media.geeksforgeeks.org/wp-content/uploads/sql-commands.jpg)

### DCL : 데이터 제어 , 접근권한 주고 뺏기(DBA들의 권한)
  - GRANT : 권한 부여
    - (예) GRANT CREATE VIEW TO _hr_ : hr에게 view를 생성할 수 있는 권한을 주겠다
  - REVOKE : 권한 해제

### DML : 데이터 조작어
  - SELECT : 데이터 검색 (DQL : 질의어)
  - INSERT : 데이터 추가
  - UPDATE : 데이터 수정
  - DELETE : 데이터 삭제(ROW 한줄씩)

### DDL : 데이터 정의어
  - DBA : table, view, sequence, PL/SQL
  - CREATE : 생성
  - DROP : 삭제(파일 삭제)
  - ALTER : 수정
  - RENAME : 이름변경
  - TRUNCATE : 잘라내기(파일 유지)

### TCL : 트랜잭션 제어(일괄처리)
  - COMMIT : 정상수행
  - ROLLBACK : 취소(*이미 저장되면 취소 못함)

## DML : 데이터 조작어
- SELECT : 데이터 검색
- INSERT : 데이터 추가
- UPDATE : 데이터 수정
- DELETE : 데이터 삭제(ROW단위)


### DML | SELECT
- 데이터베이스로부터 저장되어 있는 데이터를 검색하는데 사용됨  
```
-------------------- 필수조건 ----------------------
SELECT 전체검색 or 원하는 데이터만 검색
FROM table명(데이터가 저장된 위치)
-------------------- 옵션조건 ----------------------
[
    1. WHERE 조건문 : 일반 사용자가 요청한 데이터 검색
    2. GROUP BY 그룹컬럼 : 지정된 그룹별로 데이터 처리
       HAVING 그룹조건 : 그룹별 조건
    3. ORDER BY 컬럼명 ASC|DESC : 정렬
       (ASC : 올림차순 / DESC : 내림차순)
]
---------------------------------------------------
```

#### DML_SELECT | 필수조건

```
SELECT() FROM (table명 , view명 , SELECT~~)
```

- SELECT 뒤 ( )부분에 들어갈 내용
  - 필요한 데이터만 가져올때 : ```컬럼명1, 컬럼명2, 컬럼명3...```
  - 전체 데이터를 가져올때 : ```*```
  - SELECT는 자바에서의 for문의 기능을 한다고 볼 수 있음

- FROM 뒤에 들어갈 수 있는 내용
  - table명 , view명 , SELECT~~

#### DML_SELECT_필수조건 | 전체 가져오기

```
select * from emp;
```

#### DML_SELECT_필수조건 | 원하는 컬럼만 가져오기

```
SELECT empno, ename, hiredate, job,sal FROM emp;
```
  - || : 문자열 결합
  - 작은따옴표(' ') : 문자열 처리

```
SQL> SELECT ename ||'님의 이번달 급여는 '||sal||'입니다' FROM emp;

ENAME||'님의이번달급여는'||SAL||'입니다'
-----------------------------------------
SMITH님의 이번달 급여는 800입니다
ALLEN님의 이번달 급여는 1600입니다
```
  - 큰따옴표(" ") : 별칭주기
  
```
SQL> SELECT empno "사번", ename "이름", hiredate "입사일", sal "급여",comm as 성과급 FROM emp;
```





## DML_SELECT | 옵션조건

```
WHERE 조건(if) 
GROUP BY 그룹컬럼
HAVING 그룹 조건
ORDER BY 컬럼명(ASC|DESC)
```
  - 옵션들의 순서 틀리면 오류발생

### DML_SELECT_옵션조건 | WHERE : 조건검색(자바의 if조건문과 같은 용도)
- 형식 : ``` WHERE  컬럼명  연산자  값 ```
    - 날짜, 문자는 작은 따옴표(' ') 안에 작성해야 함
    - 숫자는 작은따옴표(' ')를 사용하지 않음
    - 값은 대소문자를 구분한다.(명령어는 구분X)

```
-- 80년도에 입사한 사람 찾기
SQL> select ename, sal,hiredate from emp where hiredate>='80/01/01' and hiredate<='80/12/31';
```

```
-- 이름중에 KING이 들어간 사람
WHERE ename='KING';
```

```
-- 이름중에 알파벳순서가 K보다 뒤인 사람 찾기
WHERE ename>'KING';
```

```
-- 이름중에 알파벳순서가 K보다 앞인 사람 찾기
WHERE ename<'KING';
```




##  WHERE문장을 사용하기 위해서 반드시 필요한 연산자
> 산술연산자 : SELECT 뒤에서 사용  
> 나머지 연산자 : WHERE 뒤에서 사용

### WHERE_연산자 | 산술연산자 : ```+``` , ```-``` , ```*``` , ```/```
- NULL값일때는 연산처리가 불가함

```
-- NULL값을 처리하지 않고 더한 경우
SQL> SELECT ename,sal,comm,sal+comm FROM emp;
ENAME                       SAL       COMM   SAL+COMM
-------------------- ---------- ---------- ----------
SMITH                       800
ALLEN                      1600        300       1900
```

```
-- NULL값을 처리하여 더한 경우
SQL> SELECT ename,sal,comm,sal+NVL(comm,0) FROM emp;
ENAME                       SAL       COMM SAL+NVL(COMM,0)
-------------------- ---------- ---------- ---------------
SMITH                       800                        800
ALLEN                      1600        300            1900
```

- ```/```
    - 0으로 나눌 경우에 오류 발생함
    - 무조건 결과값이 실수로 나온다(5/2=2.5)
    - WHERE문장 뒤에 쓰는 것이 아니라 SELECT 뒤에서 사용 (true/false로 결과가 나오는 연산이 아니기 때문에)

```
SQL> SELECT ename "이름", sal "급여" , sal*12 "연봉" FROM emp;

이름                       급여       연봉
-------------------- ---------- ----------
SMITH                       800       9600
ALLEN                      1600      19200
```

### WHERE_연산자 | 비교연산자
- 오라클에서는 비교연산자로 문자열이나 날짜를 비교할 수 있다.
  - 자바에서는 equals(),compare()를 사용하지 않으면 불가능

- ```=``` : 같다
    - 연산자 뒷부분이 문자열일 경우에 대소문자 구분
    - 연산자 뒷부분이 문자열이나 날짜일 경우에는 작은따옴표(' ')를 사용한다.
    - WHERE 문장 안에서는 비교연산자 , WHERE문장 밖에서는 **대입연산자**로 쓰임(업데이트할때)

```
-- 사원중에 급여가 3000인 사원 모두 출력하기
SQL> SELECT * FROM emp WHERE sal=3000;

-- 사원중에 급여가 1500인 사람의 이름, 입사일,급여 출력하기
SQL> SELECT ename,hiredate,sal FROM emp WHERE sal=1500;

-- 사원중에 이름이 SCOTT인 사람의 이름, 입사일,직위,급여 출력하기
SQL> select ename,hiredate,job,sal from emp where ename='SCOTT';
SQL> select ename,hiredate,job,sal from emp where ename='scott'; // 값 소문자 ERROR

-- 사원 중에 직위가 MANAGER인 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE JOB='MANAGER';

-- 입사일이 1982-12-9인 사원의 이름,입사일,급여 출력
SQL> SELECT ename,hiredate,sal FROM emp WHERE hiredate='82/12/09'; // 날짜 작은따옴표 사이에
```



- ```!=``` , ```<>``` , ```^=``` : 같지 않다.

```
-- JOB이 CLERK이 아닌 사람의 이름,직위 출력
SQL> SELECT ENAME,JOB FROM EMP WHERE JOB!='CLERK';
SQL> SELECT ENAME,JOB FROM EMP WHERE JOB<>'CLERK';
SQL>  SELECT ENAME,JOB FROM EMP WHERE JOB^='CLERK';
```



- ```<``` : 작다

```
-- 사원중에 급여가 1500보다 작은 사원의 이름,급여 출력
SQL> SELECT ENAME,SAL FROM emp WHERE sal<1500;

-- 입사일이 82/12/09보다 먼저 입사한 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE hiredate<'82/12/09'; // 날짜도 비교연산자 사용 가능 , 작은따옴표 사용

```

 - ```>``` : 크다

```
-- 입사일이 82/12/09보다 늦게 입사한 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE hiredate>'82/12/09'; // 날짜도 비교연산자 사용 가능 , 작은따옴표 사용
```

- ```<=``` : 작거나 같다

- ```>=``` : 크거나 같다
  

### WHERE_연산자 | 논리연산자 : 두 개의 조건을 비교할 때 사용함

||AND|OR|
|---|--|--|
|TT|T|T|
|TF|F|T|
|FT|F|T|
|FF|F|F|

> 자바에서는 ```&&``` , ```||```의 역할    
> 오라클에서 ```&```는 Scanner , ```||```는 문자열 결합 역할을 담당함

- ```OR``` : 둘 중에 한 개가 true면 true
- ```AND``` : 둘 다 true이면 true
    - 범위 , 기간이 포함하는 경우에 사용

```
-- 급여가 1500이상 3000이하인 사람의 모든 정보 출력
SQL> SELECT * FROM emp WHERE sal>=1500 AND sal<=3000;

-- 급여가 1500이상 또는 3000미만인 사람의 모든 정보 출력
SQL> SELECT * FROM emp WHERE sal>=1500 OR sal<=3000;
```


### WHERE_연산자 | ```NULL``` : 아예 값이 없는 것

- 값이 NULL인 경우에는 연산처리가 안된다.
  - 오라클에서 null과 연산하면 null값이 나온다 (EX. 10 + null = null)

- ```IS NULL``` : NULL값일 경우에 처리하는 연산자

```
-- 성과급이 없는 사원의 모든 정보
SQL> SELECT * FROM emp WHERE comm IS NULL;

-- 사수가 없는 직원의 이름,입사일,직위 출력
SQL> SELECT ename,hiredate,job FROM emp WHERE mgr IS NULL;
```

- ```IS NOT NULL``` : NULL값이 아닐 경우에 처리하는 연산자

```
-- 성과급이 있는 사원의 이름과 성과급 정보 출력
SQL> SELECT ename,comm FROM emp WHERE comm IS NOT NULL;
```



### WHERE_연산자 | ```IN``` : OR가 여러개일 경우에 대체하는 연산자
- ```IN``` 
  - 형식 : _칼럼명_ ```IN``` (_조건값1,조건값2,조건값3_);


```
-- 사원중에 부서가 10 또는 20인 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE deptno=10 OR deptno=20;
SQL> SELECT * FROM emp WHERE deptno IN(10,20);

-- 사원중에 JOB이 MANAGER OR CLERK인 직원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE job IN('MANAGER','CLERK');
SQL> SELECT * FROM emp WHERE job IN('MANAGER'OR'CLERK'); // OR 쓰면 IN과 중복 error

-- 직원중에 hiredate가 '81/06/09','81/11/17','82/01/23'인 사람의 모든 정보 출력
SQL> SELECT * FROM emp WHERE hiredate IN('81/06/09','81/11/17','82/01/23');
```

- 부정형 : _칼럼명_ ```NOT IN``` (_조건값1,조건값2,조건값3_);

```
-- 사원중에 부서가 10 또는 20이 아닌 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE deptno NOT IN(10,20);
```



### WHERE_연산자 | ```BETWEEN ~ AND ~ ``` : 기간이나 범위를 구할 때 사용
- 속도가 비교연산자보다 느림
- 문자열, 날짜도 처리 가능함

```
// JAVA
sal>=100 && sal<=3000
// ORACLE
sal BETWEEN 100 AND 3000
sal>=100 AND sal<=3000
```

```
-- 1981년에 입사한 모든 사원의 정보를 출력
SQL> SELECT * FROM emp WHERE hiredate BETWEEN '81/01/01' AND '81/12/31'; // 날짜도 포함해서 처리 가능

-- 알파벳순으로 정렬했을때 'ADAMS'와 'KING'사이에 있는 이름 출력
SQL> SELECT ename FROM emp WHERE ename BETWEEN 'ADAMS' AND 'KING'; // 문자도 포함해서 처리 가능
```

- 부정형 : ```NOT BETWEEN ~ AND ~ ```

```
-- 1981년에 입사하지 않은 모든 사원의 정보를 출력
SQL> SELECT * FROM emp WHERE hiredate NOT BETWEEN '81/01/01' AND '81/12/31';
```



### WHERE_연산자 | ```LIKE``` : 유사문자열 찾기
- 포함문자, 시작문자 , 끝 , 글자수 찾는 경우에 사용
- 형식 : ```WHERE``` _컬럼명_ ```LIKE``` ```'%``` _찾을문자_ ```%'```  

- ```%``` : 문자열 , 찾을 문자의 글자수를 모르는 경우
  - 'A%'    : A로 시작하는 모든 문자열     (자바 : startsWith()와 유사)
  - '%A'    : A로 끝나는 모든 문자열       (자바 : endsWith()와 유사)
  - '%A%'   : A를 포함하고 있는 모든 문자열 (자바 : contains()와 유사)
  - '%' || ? || '%' : 무슨데이터 들어갈지 모르는 경우(1개)
  - '%' || ?,? || '%' : 무슨데이터 들어갈지 모르는 경우(2개)  
[like 연산자의 변수 처리 및 예제](https://oingbong.tistory.com/112)  
[?의 역할 : preparedStatement](https://cocodo.tistory.com/11)  
[LIKE와 preparedStatement](http://cocagolau.blogspot.com/2014/04/sql-prepared-statement-like.html)  


```
-- 이름이 A로 시작하는 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE ename LIKE 'A%';

-- 이름이 S로 끝나는 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE ename LIKE '%S';

-- 이름이 K를 포함하는 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE ename LIKE '%K%';

-- 동에서 신촌을 포함하는 동 출력
SQL> SELECT dong FROM zipcode WHERE dong LIKE '%신촌%';

-- 82년에 입사한 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE hiredate LIKE '82%';
```


- ```_``` : 한글자(한글,숫자,알파벳 전부 가능) , 글자수를 아는 경우
  - '_A'    : A로 끝난 2글자의 문자열
  - '__C__' : 가운데 C가 들어간 5글자의 문자열

```
-- 이름이 5글자이고, 가운데 O가 있는 사원의 모든 정보 출력
SQL> SELECT * FROM emp WHERE ename LIKE '__O__';

-- 이름이 4글자인 사람의 모든 정보 출력
SQL> SELECT * FROM emp WHERE ename LIKE '____';
```









### DML_SELECT_옵션조건 | ```GROUP BY``` : 그룹별 처리

### DML_SELECT_옵션조건 | ```HAVING``` : 그룹 조건
  - GROUP BY가 쓰이지 않았다면 존재할 수 없음

### DML_SELECT_옵션조건 | ```ORDER BY``` : 정렬
- 순서 : 맨 마지막 줄에 쓰임
- 형식 : ```ORDER BY``` _컬럼명_ ```ASC```or```DESC```
- 속도가 느려서 주로 INDEX사용
- 오라클 기본 출력순서는 데이터가 저장된 순서임
- 오라클 번호는 0번이 아닌 1번부터 시작됨  

- ```DESC``` : 내림차순

```
-- 급여순으로 내림차순 정렬
SQL> SELECT ename,sal FROM emp ORDER BY sal DESC;

ENAME                       SAL
-------------------- ----------
KING                       5000
FORD                       3000

-- 2중정렬 : sal(2)이 같은 값이 존재하면 그 부분만 ename(1)순으로 재정렬
SQL> SELECT ename,sal FROM emp ORDER BY sal DESC,ename DESC;
SQL> SELECT ename,sal FROM emp ORDER BY 2 DESC,1 DESC;

-- 2중정렬 : 부서 번호별로 정렬 + 부서 내에서 급여별로 재정렬
SELECT ename,sal,deptno FROM emp ORDER BY deptno ASC,sal DESC;

```


- ```ASC``` : 오름차순(디폴트 , ORDER BY만 쓰고 생략가능)

```
-- 급여순으로 오름차순 정렬
SQL> SELECT ename,sal FROM emp ORDER BY sal ASC;
SQL> SELECT ename,sal FROM emp ORDER BY sal; // ASC는 생략 가능
SQL> SELECT ename,sal FROM emp ORDER BY 2;   // sal 대신 2로 입력해도 됨(ename은 1)

ENAME                       SAL
-------------------- ----------
SMITH                       800
JAMES                       950
ADAMS                      1100
```

## 집합연산자
- 컬럼이 같아야 사용할 수 있음

- ```UNION```
  - 합집합
  - 중복값(교집합) 제거

- ```UNION ALL```
  - A+B
  - 중복값(교집합) 제거 X

- ```INTERSECT```
  - 교집합
- ```MINUS A-B```
  - 차집합
- ```MINUS B-A```




## 기타
- ```DISTINCT``` : 중복된 데이터를 제외하고 출력

```
SQL> SELECT DISTINCT job FROM emp;
SQL> SELECT DISTINCT deptno FROM emp;
```

- ```&``` : 입력하시오

```
SQL> SELECT ename FROM emp WHERE empno=&no;
Enter value for no: 7788
old   1: SELECT ename FROM emp WHERE empno=&no
new   1: SELECT ename FROM emp WHERE empno=7788

ENAME
--------------------
SCOTT
```
- ```AND``` : 자바에서의 & 기능
- ```LIKE``` : 자바에서의 contains
- ```--``` : 한줄 주석
- ```/* */``` : 여러줄 주석

## 예시
> 검색어 입력받아서 해당 문자열 포함하는 데이터 찾기

```JAVA
// 검색어 입력 받기
Scanner scan = new Scanner(System.in);
System.out.println("검색어 입력 : ");
String data = scan.next();

// SQL문장 전송
String sql="SELECT ename FROM emp WHERE ename LIKE '%'||?||'%'";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setString(1, data);
```
