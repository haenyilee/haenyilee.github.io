# Oracle_SQL과 PL SQL

## [0장](https://github.com/haenyilee/Oracle-Study/wiki/Oracle_00%EC%9E%A5_%EA%B0%9C%EC%9A%94-%EB%B0%8F-%ED%8A%B9%EC%A7%95) 공부를 시작하기 전에 미리 알아두세요
1. 미리 알아야 할 몇 가지 중요한 개념
2. 오라클 데이터베이스 서버에 접속하기

## [1장](https://github.com/haenyilee/Oracle-Study/wiki/Oracle_01%EC%9E%A5_DML_SELECT) SELECT 명령을 이용하여 데이터를 조회합니다
1. 모든 컬럼 조회하기
2. 원하는 컬럼만 조회하기
3. SELECT 명령에 표현식을 사용하여 출력하기
4. 컬럼 별칭 사용하여 출력하기
5. DISTINCT 명령어로 중복된 값을 제거하고 출력하기
6. 연결 연산자로 컬럼을 붙여서 출력하기
7. 원하는 조건만 골라내기 - WHERE 절 사용
8. SQL에서 기본 산술 연산자 사용하기
9. 다양한 연산자를 활용하는 방법
10. 정렬하여 출력하기 - ORDER BY 절 사용하기
11. 집합 연산자

## [2장](https://github.com/haenyilee/Oracle_Basic/wiki/Oracle_SQL_단일행함수) SQL 단일행 함수를 배웁니다
1. 문자 함수
2. 숫자 관련 함수들
3. 날짜 관련 함수들
4. 형 변환 함수
5. 일반 함수
6. 정규식(Regular Expression) 함수로 다양한 조건 조회하기
7. 11g에서 추가된 정규식 함수

## [3장](https://github.com/haenyilee/Oracle_Basic/wiki/Oracle_SQL_%EC%A7%91%ED%95%A9%ED%95%A8%EC%88%98(%EB%8B%A4%EC%A4%91%ED%96%89%ED%95%A8%EC%88%98)) SQL 복수행 함수(그룹 함수)를 배웁니다
1. GROUP 함수의 종류
2. GROUP BY 절을 사용해 특정 조건으로 세부적인 그룹화하기
3. HAVING 절을 사용해 그룹핑한 조건으로 검색하기
4. 반드시 알아야 하는 다양한 분석 함수들

## [4장](https://github.com/haenyilee/Oracle_Basic/wiki/Oracle_SQL_JOIN) JOIN을 배웁니다
1. Cartesian Product(카티션 곱)
2. EQUI Join(등가 조인)
3. Non-Equi Join(비등가 조인)
4. OUTER Join(아우터 조인)
5. SELF Join

## 5장 DDL 명령과 딕셔너리를 배웁니다
1. CREATE - 새로 생성하라
2. ALTER 명령
3. TRUNCATE 명령
4. DROP 명령
5. DELETE, TRUNCATE, DROP 명령어의 차이점 비교
6. 11g에서 추가된 기능 소개
7. Data Dictionary(데이터 딕셔너리)

## 6장 DML로 데이터를 관리하는 방법을 배웁니다
1. INSERT(새로운 데이터 입력하기)
2. UPDATE(데이터 변경하기)
3. DELETE
4. MERGE
5. UPDATE 조인
6. TRANSACTION 관리하기

## 7장 Constraint(제약조건)를 배웁니다
1. 제약 조건의 종류
2. 제약 조건 사용하기
3. 제약 조건 관리하기

## 8장 INDEX(인덱스)를 배웁니다
1. 인덱스(INDEX)란 무엇일까요?
2. 인덱스의 생성 원리
3. 인덱스 구조와 작동 원리(B-TREE 인덱스 기준입니다)
4. 인덱스의 종류
5. 인덱스의 주의사항
6. 인덱스 관리 방법
7. Invisible Index(인비저블 인덱스) -11g New Feature

## [9장](https://github.com/haenyilee/Oracle-Study/wiki/Oracle_09%EC%9E%A5_VIEW(%EB%B7%B0)) VIEW(뷰)를 배웁니다
1. 단순 뷰(Simple View)
2. 복합 뷰(Complex View)
3. Inline View(인라인 뷰)
4. View 조회 및 삭제하기
5. Materialized View(MVIEW) - 구체화된 뷰

## 10장 Sub Query(서브 쿼리)를 배웁니다
1. Sub Query가 무엇일까요?
2. Sub Query의 종류
3. Scalar Sub Query(스칼라 서브 쿼리)

## 11장 SEQUENCE(시퀀스)와 SYNONYM(동의어)를 배웁니다
1. SEQUENCE(시퀀스)
2. SYNONYM(시노님 - 동의어)

## 12장 계층형 쿼리(Hierarchical Query)를 배웁니다
1. 계층형 쿼리의 문법
2. 계층형 쿼리의 기본 구조
3. 계층 구조에서 일부분만 계층화하기
4. CONNECT_BY_ISLEAF( ) 함수
5. CONNECT_BY_ROOT 함수

## 13장 오라클 계정 관리 방법을 배웁니다
1. User와 Schema(스키마)에 대해서 알아봅니다
2. PROFILE(프로파일) 생성 및 관리하기
3. PRIVILEGE(권한) 관리에 대해 배웁니다
4. Role(롤) 관리하기

## 14장 12c SQL에 추가된 새로운 기능
1. DEFAULT value로 sequence의 next value 지정 가능
2. Invisible Column 사용 가능
3. 순위 뽑을 때 Top-N 기능 사용 가능
4. IDENTITY Column 지원
5. Null 값을 위한 DEFAULT 값 지정 가능
6. 그 외 주요 New Features

## 15장 Oracle PL/SQL에 입문합니다
1. PL/SQL이란
2. PL/SQL의 런타임 구조
3. PL/SQL 기본 구조
4. PL/SQL BLOCK 기본 구성
5. PL/SQL 블록 작성 시 기본 규칙과 권장사항
6. PL/SQL 문 내에서의 SQL 문장 사용하기
7. PL/SQL에서의 렉시칼
8. PL/SQL에서의 블록 구문 작성 지침
9. 중첩된 PL/SQL 블록 작성하기
10. PL/SQL에서의 연산자 사용하기
11. PL/SQL에서 변수의 의미와 사용법
12. PL/SQL 제어문 사용법
13. PL/SQL Cursor(커서)
14. ORACLE EXCEPTION(예외 처리)
15. ORACLE SUBPROGRAM