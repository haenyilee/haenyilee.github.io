---
sort: 15
---

# PL,SQL

## PL/SQL이란
- PROCEDURE, FUNCTION, PACKAGE , TRIGGER를 제작할 때 사용하는 언어이다.
- 사용자 정의로 사용한다.
- 재사용하기 위함이 목적이다.


## PL/SQL 기본 구조
- PROCEDURE : 리턴형이 없는 자바스크립트 언어와 같음
- FUNCTION : 리턴형이 있는 함수
  - 함수 : 독립(C언어)
  - 메소드 : 클래스 종속
- PACKAGE : 관련된 PROCEDURE , FUNCTION 을 모아서 둔 곳
- TRIGGER : 이미 지정된 이벤트 발생시에 자동 처리


## PL/SQL BLOCK 기본 구성



- 형식)

```
선언부
  변수 선언
구현부
  SQL문장
예외처리부 (생략이 가능)
```

- 변수선언
- 제어문
- SQL 구현방식
- 예외처리


## PL/SQL에서 변수의 의미와 사용법
### 변수
- 지역변수
- 매개변수 : SUBSTR('',1,2) MAX(컬럼명)

- 스칼라변수 : 일반 변수(NUMBER,VARCHAR2,CLOB,DATE)
  - id VARCHAR2(10)
  
```ORACLE
SET SERVEROUTPUT ON;
DECLARE
    vempno NUMBER(4);
    vename VARCHAR2(20);
    vjob VARCHAR2(20);
    vhiredate DATE;
    vsal NUMBER(7,2);
BEGIN
    SELECT empno,ename,job,hiredate,sal INTO vempno,vename,vjob,vhiredate,vsal
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vempno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vjob);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vhiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vsal);
END;SET SERVEROUTPUT ON;
DECLARE
    vempno NUMBER(4);
    vename VARCHAR2(20);
    vjob VARCHAR2(20);
    vhiredate DATE;
    vsal NUMBER(7,2);
BEGIN
    SELECT empno,ename,job,hiredate,sal INTO vempno,vename,vjob,vhiredate,vsal
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vempno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vjob);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vhiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vsal);
END;
```  
  
 
- %TYPE : 원형 (다른 테이블의 원형)
  - emp.ename%TYPE => ename이 가지고 있는 데이터형을 가지고 온다.

```oracle   
SET SERVEROUTPUT ON;
DECLARE
    vempno emp.empno%TYPE;
    vename emp.ename%TYPE;
    vjob emp.job%TYPE;
    vhiredate emp.hiredate%TYPE;
    vsal emp.sal%TYPE;
BEGIN
    SELECT empno,ename,job,hiredate,sal INTO vempno,vename,vjob,vhiredate,vsal
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vempno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vjob);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vhiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vsal);
END;SET SERVEROUTPUT ON;
DECLARE
    vempno NUMBER(4);
    vename VARCHAR2(20);
    vjob VARCHAR2(20);
    vhiredate DATE;
    vsal NUMBER(7,2);
BEGIN
    SELECT empno,ename,job,hiredate,sal INTO vempno,vename,vjob,vhiredate,vsal
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vempno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vjob);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vhiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vsal);
END;
```

- %ROWTYPE
  - emp%ROWTYPE => emp테이블이 가지고 있는 모든 컬럼의 데이터형을 읽어 온다.

```oracle
SET SERVEROUTPUT ON;
DECLARE
    vemp emp%ROWTYPE;
BEGIN
    SELECT * INTO vemp
    FROM emp
    WHERE empno=7788;
    -- 출력
    DBMS_OUTPUT.PUT_LINE('======결과======');
    DBMS_OUTPUT.PUT_LINE('사번:'||vemp.empno);
    DBMS_OUTPUT.PUT_LINE('이름:'||vemp.ename);
    DBMS_OUTPUT.PUT_LINE('직위:'||vemp.job);
    DBMS_OUTPUT.PUT_LINE('입사일:'||vemp.hiredate);
    DBMS_OUTPUT.PUT_LINE('급여:'||vemp.sal);
END;
```


- RECORD : VO와 같다. 여러개의 테이블 컬럼을 받아서 처리한다. 

- CURSOR : ArrayList와 같다. 여러개의 ROW처리가 가능하다.
  - 스칼라변수 , %TYPE , %ROWTYPE , RECORD 는 한 개의 ROW만 처리 가능하다.



- 변수 사용과 출력



```TIP
**출력함수**
- DBMS_OUTPUT.PUT_LINE() : System.out.println()
- DBMS_OUTPUT.PUT() : System.out.print()
```



## PL/SQL의 런타임 구조
## PL/SQL 블록 작성 시 기본 규칙과 권장사항
## PL/SQL 문 내에서의 SQL 문장 사용하기
## PL/SQL에서의 렉시칼
## PL/SQL에서의 블록 구문 작성 지침
## 중첩된 PL/SQL 블록 작성하기
## PL/SQL에서의 연산자 사용하기



## PL/SQL 제어문 사용법
## PL/SQL Cursor(커서)
## ORACLE EXCEPTION(예외 처리)
## ORACLE SUBPROGRAM
