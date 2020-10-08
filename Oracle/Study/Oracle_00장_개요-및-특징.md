# Run SQL Command Line
- ```conn``` : 사용자 계정에 접근
```
SQL> conn hr/happy
Connected.
```
- ```엔터``` : 저장
- ```ed``` _파일명_ : 편집기 열기
- ```FROM DUAL``` : 원본데이터 없이 연습할 때
- ```DESC``` _파일명_ : 구조 확인 , 컬럼명 확인
```
SQL> DESC dept
 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 DEPTNO                                             NUMBER(2)
 DNAME                                              VARCHAR2(14)
 LOC                                                VARCHAR2(13)
```
- ```@``` _파일경로_ : 편집기파일 안에 있는 코드 수행
  - 관리자권한으로 실행하지 않으면 다른 곳에 저장됨
  - 관리자권한으로 실행하면 [oraclexe-product- server - bin]에 저장됨
   - SAVE zipcode.txt APPEND;
- ```set linesize``` _숫자_ : 너비 조절
- ```set pagesize``` _숫자_ : 한페이지에 숫자만큼 row가 배치되도록 설정(디폴트 : 14줄이 1페이지임)

# 오라클의 단점
- 오류가 나도 다음문장을 실행함(비절차적언어)
  - 자바 : 오류가 나면 중간에 종료함
- 정형화된 데이터임 : 읽어들이지 못하는 데이터가 많음(& 등)

# 오라클 주의점
- 오라클은 정렬이 안된 상태임 => 오라클은 저장된 순서로 읽어오기 때문이다.
- 오라클은 대소문자를 구분하지 않는다.
- 날짜, 문자는 작은 따옴표(' ') 안에 작성해야 한다.
  - 숫자는 작은따옴표(' ')를 사용하지 않는다.


# 오라클 특징

- ajax(비동기화 자바스크립트 and XML) : 가상메모리에 들어가있어서 파싱불가능(해킹뿐)

- Oracle은 Database의 중개자 : RDMS
- Oracle은 중형 DB

- Oracle의 저장구조를 알아야 제어할 수 있다.
  - 일반 변수는 Stack(낱개구조), Array(연속적 구조), Class(병렬 구조)로 저장됨
  - 오라클은 Table형태로 저장됨

- 오라클의 Table은 일반 파일이라고 생각하면 됨
  - 일반 : [폴더]안에 [파일] 저장 => 제어가 어려움
  - 오라클 : [데이터베이스]안에 [테이블]저장 => 제어가 쉬움
    - 오라클깔면 [XE]라는 전역데이터베이스가 자동 생성됨
    ```JAVA
    // 오라클 주소
    String url="jdbc:oracle:thin:@localhost:1521:XE";
    ```
      - 오라클 데이터는 인터넷에 저장되어있다고 생각하면 쉬움, 그래서 URL통해서 매번 연결해야함

- 오라클의 Table형태는 2차원임
![](https://lh3.googleusercontent.com/proxy/ZhV3lLkvVIw2sD2M1WL27r4bGay2YGG8lmhD1NfuVc-sZxkAdUY-OmLG0nkIpJpNEa0xSdQsJ4TrixeX5v69dZh_EwUTycbNDWHq2Fx4YA1b6J51WUX3mY6HlvkoTs8GpeY43JWwlWnloI19X28t1zPX_mEm9slvT1WSVEx1AWS5IbANsotOZqsERgwGAQKOXLO8A64)
  - 맨윗줄 : Column 
    - 자바에서는 멤버변수의 기능을 수행함
  - 행(1행제외) : Row, Record, Tuple(*튜플여러개 = cardinality) 
     => row가 제어 단위임(한줄이 세트로 움직임)
  - 열(1열제외) : 도메인

- 유형
  - VARCHAR2(10) : 가변길이 문자데이터, 10자리까지의 글자
  - NUMBER(4) : 숫자데이터 , 0~4자리까지의 정수
  - NUMBER(7,2) : 숫자데이터 , '전체 7자리 + 소수점 이하 2자리'의 실수 데이터
  - DATE : 날짜 형식


# 오라클의 데이터형
```
// 오라클 데이터형
SQL> DESC dept
 Name                                      Null?    Type
 ----------------------------------------- -------- ----------------------------
 DEPTNO                                             NUMBER(2)
 DNAME                                              VARCHAR2(14)
 LOC                                                VARCHAR2(13)
```

``` java
// 자바로 변형
class DeptVO
{
    int deptno;
    String dname;
     String loc;   
}

```


### 문자형
- 자바 : String

|유형| 오라클|자바|
|---|------|------|
|고정|     CHAR     | List | 
|가변|VARCHAR , CLOB| ArrayList|


- CHAR(1~2000byte)
  - 한글은 2byte(자음+모음)이기 때문에 ```CHAR(4)```는 한글 두글자만 저장 가능
  - 고정형이기 때문에 지정해둔 만큼 메모리크기를 무조건 할당함
  - 글자수가 일정할 때 주로 사용
- VARCHAR2(1~4000byte)
  - 가변형이기 때문에 들어온 갯수만큼 메모리 지정해줌
  - 가장 많이 쓰이는 데이터형
- CLOB(~4GB)
  - 많은 양의 데이터 저장이 가능하기 때문에 줄거리, 뉴스, 자기소개 등에 쓰이는 데이터형임
  - 가변형이기 때문에 들어온 갯수만큼 메모리 지정해줌

### 숫자형
- 자바 : int, double
- NUMBER(4) 
  - 정수형
  - 38자리까지 저장 가능함
- NUMBER(7,2) 
  - 정수 OR 실수형
  - ( ) 안 뒤에 숫자는 소수점 자리수 
- NUMBER
  - 14자리 정수까지 저장 가능함

### 날짜형
- 자바 : java.util.Date
- DATE : 일반,날짜,시간
- TIMESTAMP
  - 운동 경기할때 주로 사용되는 시간단위

### 기타형
- 자바 : java.io.InputStream

- BFILE
  - 4GB까지 저장할수 있음
  - 파일형식으로 저장됨
- BLOB
  - 4GB까지 저장할수 있음
  - 바이러리로 저장됨
  - 증명사진, 동영상, 그림파일에 주로 사용됨

# SQL문 작성 방법
- 대소문자 구분X
  - 저장된 데이터는 대소문자를 구분하지만 명령어는 대소문자 구분하지 않음
  * 일반적으로 키워드는 대문자로 입력함
  * 다른 모든 언어, 테이블이름, 열이름은 소문자로 입력(권장)
```
SELECT ename FROM emp WHERE ename LIKE '%A%';
SELECT ename FROM emp WHERE ename LIKE '%a%';
```
- 여러 줄에 입력할 수 있음
- 마지막 절의 끝에 ';'을 통해 명령이 끝났음을 표시함
