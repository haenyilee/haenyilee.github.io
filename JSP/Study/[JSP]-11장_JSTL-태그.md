
# JSTL 태그

## maven라이브러리 다운로드
- 라이브러리를 관리하는 프로그램이 maven임
- JSTL : https://mvnrepository.com/artifact/javax.servlet/jstl/1.2
- Standard : https://mvnrepository.com/artifact/taglibs/standard/1.1.2


## JSTL(Java Standard Tag Library)
- XML로 제작하기 때문에 여는태그, 닫는태그, 따옴표 작성에 유의해야 한다.
- 지원하는 태그와 속성만 사용이 가능하다
- 오버라이딩을 통한 사용자 정의 태그 라이브러리를 사용할 수 있다.

## JSTL의 종류
- core : 변수 설정 , 제어문 , URL , 파일 이동
- format : 문자변환 ,  날짜변환 , 숫자변환
- fn : 문자열함수 , 컬렉션함수를 제어
- xml : XML파싱(manager.java)
- sql : 오라클 연결

## 태그 라이브러리 선언
```<%@ taglib prefix="" uri="" %>```
-  태그(xml)을 이용해서 자바 라이브러리를 만든 것이다. 

#### 태그 라이브러리 종류
- ```<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>``` : 변수설정 , 제어문 , URL
- ```<%@ taglib prefix=fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>``` : 날짜 변경 , 숫자 변경
- ```<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>``` : String class lib

## core
#### 1. 변수 선언

- ```<c:set var="키" value="값">```
- ```request.setAttribute("키",값)```와 같은 코딩이다.
- var는 key , value에는 데이터 값이 들어간다.



#### 2. 제어문
#### (1) ```<c:if test="조건문">결과값</c:if>``` <br>
- test : 조건을 저장하는 속성
- XML은 없는 속성을 사용하면 error발생한다.
- else를 나타내는 속성이 없다.
  - 일반적으로 다중 if문, 선택조건문을 사용할 때는 <c:choose>를 사용한다.

#### (2) ```<c:for var="i" begin="1" end="10" step="1">```
- ```for(int i=1;i<=10;i++)```와 같은 코딩이다.
- var : 변수명
- begin : 초기값
- end : 조건
  - ```<=```이 포함되어 있다.
- step : 증가식
  - 음수는 사용할 수 없다.
    - 즉, 증가는 가능하지만, 감소는 불가능 하다.
  - step=1은 생략이 가능하다.

#### (3) ```<c:forEach var="vo" items="list">```
- items : 배열이나 컬렉션이 사용된다.

```
<c:forEach var="i" begin="1" end="10" step="1">
	${i }&nbsp;
</c:forEach>
```

#### (3).2 foreach 속성
- ${status.current}    <!- 현재 아이템 ->
 
- ${status.index}        <!- 0 부터의 순서 ->
 
- ${status.count}        <!- 1 부터의 순서 ->
 
- ${status.first}        <!- 현재 루프가 처음인지 반환 ->
 
- ${status.last}        <!- 현재 루프가 마지막인지 반환 ->
 
- ${status.begin}        <!- 시작 값 ->
 
- ${status.end}        <!- 끝 값 ->
 
- ${status.step}        <!- 증가 값 ->

#### (4) ```<c:forTokens var="s" values="red,green,blue,black,yellow" delims=",">```
- values : 자를 문자열
- delims : 구분 문자



#### (5) switch문장
```
<c:choose>
	<c:when test=="조건">처리문장</c:when>
	<c:when test=="조건">처리문장</c:when>
	<c:otherwise>디폴트</c:otherwise>		
</c:choose>
```

#### 3. 파일이동
(1) ```<c:redirect url="이동할 주소">``` <br>
- ```response.sendRedirect("")```

(2) ```<c:import>``` <br>
- ```<c:set>```
- ```<c:forEach>```
- ```<c:if>``` : ```<c:else>```는 없다.
- ```<c:choose>```
- ```<c:redirect>```

#### 4. 출력
#### 4.1 ```<c:out value="">```
- 출력용이다.

## format
#### 1. 날짜변환 : ```<fmt:formatDate value="변경될 날짜 데이터" pattern="">```
- 년도 : yyyy
- 월 : M , MM
- 일 : dd
- 시간 : hh
- 분 : mm
- 초 : ss

#### 2. 숫자변환 : ```<fmt:formatNumber value="변경할 숫자" pattern="00,000">```
- 오라클에서는 숫자 패턴에 9를 사용하는 반면, 자바에서는 0을 사용한다.

#### 3. function(fn) : String메소드 호출
- 종류 : 
length(), 
substring(), 
split(), 
toUpperCase(), 
toLowerCase(), 
replace(), 
indexOf(), 
startsWith(), 
endsWith()

- 사용 예시
${fn:length("abcdefg")}
${fn:substring()}

## 실습해보기
#### 1~10까지 짝수만 출력하기

```jsp
<c:forEach var="i" begin="1" end="10">
	<c:if test="${i%2==0 }">
		${i }&nbsp;
	</c:if>
</c:forEach>
```

#### 1~10까지 홀수만 출력하기

```jsp
<c:forEach var="i" begin="1" end="10">
	<c:if test="${i%2 ne 0 }">
		${i }&nbsp;
	</c:if>
</c:forEach>
```

