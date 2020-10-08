---
sort: 2
---

# MVC구조

- M : JAVA
- V : JSP
- C : Servlet

# EL태그
- el : Express Language
- 화면을 출력할때 사용한다.
- 실무에서는 자바와 HTML을 분리해서 사용한다.
- HTML만으로는 자바에서 가지고 오는 데이터를 출력할 수 있는 기능이 없기 때문에, 출력할 수 있는 기능을 제공해주는 대표적인 프로그램이 EL이다.
- 제어문이 HTML에는 존재하지 않기 때문에 태그 형식으로 제어문을 제공하는 것을 JSTL이라고 한다.
  - JSTL : Java Standard Tag Library 
- EL + JSTL = 스프링에서 출력용으로 사용하는 프로그램
- 스프링은 데이터베이스와 기타 자바 기능을 관리하고 출력은 JSP가 담당한다. 이때 JSP에서 사용되는 것이 EL과 JSTL이다.
- EL을 사용하게 되면 ```<%= %>```이 사라지고 ```${}```를 사용한다.
- JSP 출력 태그 종류 : ```<%= %>``` , ```out.println```
- 선언문( ```<%@ %>``` )은 사라지지 않는다.
- ```${자바의 일반변수}``` (X) , ```${requestScope.key명}``` (O)

## EL 표현식
- ```<% request.getParameter() %>``` 대신 ```${key명칭}```을 사용하는 것을 EL표현식이라고 한다.
- request, session, application에 데이터 값을 담아줘야 한다.
- {}사이에는 key이름이 들어가야 한다.
- key를 입력하면 결과값을 출력한다. 
- 변수명을 작성하지 않도록 유의한다.
- (request/session/application)Scope. 부분은 생략이 가능하다.
- 이때 key명칭이 중복된다면 1. request ▶ 2. session ▶ 3. application 순으로 값을 출력한다.

- 문자열 결합 : ```${i+="문자열"}```
  - ```${i}문자열``` 으로 보통 작성한다.

#### 1. ```${requestScope.id}```
- ```request.getAttribute("id")```와 같은 코딩
- 한 개의 JSP에서 사용이 가능하다.
- 한 번 사용하고 버리는 경우에 ```request.setAttribute()```

#### 2. ```${sesisonScope.id}```
- ```sesison.getAttribute("id")```와 같은 코딩
- 프로젝트 내 모든 JSP에서 사용이 가능함
- 여러 개의 JSP에서 공통으로 사용되는 데이터가 있는 경우에 ```session.setAttribute()```

#### 3. ```${applicationScope.id}``` 
- ```application.getAttribute("id")```와 같은 코딩


#### 4. ```${parm.id}``` = ```request.getParameter("id")```
- set/getParameter는 사용자가 요청한 데이터를 받을 때 사용한다.
- set/getAttribute는 사용자가 요청한 데이터 외의 다른 데이터를 받을 때 사용한다.

```jsp
<h1>request에 있는 데이터 출력</h1>
이름(방식1):<%=request.getAttribute("name") %> <br>
이름(방식2):${requestScope.name} <br>
이름(방식3):${name} <br>

성별:${sex }<br>

<h1>session에 있는 데이터 출력</h1>
이름(방식2):${sessionScope.name1} <br>
이름(방식3):${name1} <br>

성별:${sex1 }<br>

<h1>application에 있는 데이터 출력</h1>
```

## EL에서 사용하는 연산자 
- 제어문(조건문) 사용 시에 필요하다.
#### 1. 산술연산자
#### (1) ```+```
- 순수하게 숫자만 계산한다 (문자열 결합의 기능은 없다!)
- 문자열이 있는 경우 자동으로 숫자형으로 변환한다(```Integer.parseInt()```)
- null이 있는 경우 0으로 취급한다.
```JSP
${10+10}=20
${10+"10"}=20
${null+10}=10
${10+"10 "}=오류
${10+="10"}=1010
```

#### (2) ```-```
```JSP
${10-10}=0
${10-"10"}=0
${null-10}=-10
```

#### (3) ```/```
- 0으로 나눌 수 없다
- 정수/정수=실수
- div를 사용할 수 있다.
```JSP
${5/2}=2.5
${5 div 2}=2.5
```

#### (4) ```%```
- 나머지를 구한다
- mod라고 사용할 수 있다.
```JSP
${5%2}=1
${5 mod 2}=1
```

#### 2. 비교연산자
- 결과값이 true / false
- 조건문에 사용된다.

#### (1) ```==```
- 문자열도 ==으로 비교한다.
- eq로 대체 사용할 수 있다.

#### (2) ```!=```
- 문자열도 !=으로 비교한다
- ne로 대체 사용할 수 있다.

#### (3) ```<=```
```jsp
${7<=8}
${7 le 8}
```

#### (4) ```>=```
```jsp
${7<=8}
${7 ge 8}
```

#### (5) ```< , >```
- < 는 lt이다.
- > 는 gt이다.
```jsp
${7<8}
${7 lt 8}
```


#### 3. 논리연산자
#### (1) ```&&``` 
- and로 대체사용 가능하다

#### (2) ```||```
- or로 대체사용 가능하다

#### (3) ```!```
- not으로 대체사용 가능하다


#### 삼항연산자
```jsp
<c:set var="sex" value="1"/>
${sex==1?"남자":"여자" }
<!-- 결과값 : 남자 -->
```


#### 문자열 결합
```jsp
<c:set var="msg1" value="Hello"/>
<c:set var="msg2" value="JSP(JSTL,EL)"/>
${msg1+=msg2 }
```
