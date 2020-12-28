---
sort: 2
---

# CRUD게시판 - 영화 상영 목록

# 초기 셋팅

## main.jsp에서 mode7 include하기
- 사용자가 요청할때마다 jsp파일을 변경해주는 역할 수행하므로, <br>
Controller역할을 수행한다고 볼 수 있다.

## movie폴더에 home.jsp생성

## movie-mapper.xml파일 생성하기
- SQL문장이 작성되는 공간이다.

## config.xml에 생성된 매퍼 등록하기
```
<mapper resource="com/sist/dao/movie-mapper.xml"/>
```

## MovieVO만들기

## MovieDAO만들기
- static block을 주는 이유 : 자동 초기화가 될 수 있게 하기 위함이다.
- xml에 주석이 한글로 되어 있기 때문에 2바이트씩 읽어올 수 있는 Reader로 읽어와야한다.
- config.xml먼저 읽고 파싱을 한다.
  - Config.xml에 movie-mapper.xml은 꼭 등록되어 있어야<br>
data-board-mapper와 구분된다

- build라는 메서드가 파싱하는 메서드이다.
  - 이때 태그를 하나씩 읽어서 데이터만 추출하는 SAX파싱법을 수행한다.

- mybatis가 제공해준 태그와 속성을 반드시 사용해야 한다.
  - dtd에 태그와 속성이 정의되어 있다.

- XML을 이용하는 목적?
  - 마이바티스에 처리를 요청하는 내용들을 xml파일로 작성한다.
  - 자바 소스를 제공하지 않지만, 동작에 필요한 데이터를 XML에 올려주면 읽어가기 때문이다.
  - 라이브러리이기 때문에 컴파일된 소스만 보내주기 때문에 자바 소스를 공개하지 않는다.
  - 모든 라이브러리는 소스를 공개하지 않고 실행할 수 있는 기계어형식의 파일만 제공한다.
  - 컴퓨터가 읽어들이는 형식인 기계어, 즉 .class 파일형식만 넘겨준다(.java파일 제공 X)
  - 실무에서 배포할때도 자바 소스는 제공하지 않는다. 컴파일된 .class파일을 보내준다.
  - 배포할 때, war(웹을 묶어준것)파일 사용함 (*jar=자바를 묶어둔 것)
 



# 영화 목록 출력하기

## mapper에서 SQL문장 작성하기
(1) 영화 리스트 데이터 불러오기
- resultType에 vo를 등록한다.
  - 미리 등록할 수도 있다.

(2) 전체 페이지 구하기

(3) 카테고리별 데이터 가져오기

## DAO에서 실행하고 값 받기
- 자동으로 컬럼명과 VO의 변수명을 매칭해서 값을 넘겨준다. <br>
(1) 영화 리스트
(2) 전체 페이지
(3) 카테고리별 데이터

## home.jsp에서 화면에 출력하기
(1) Java
- 사용자 요청값 받기
  - page는 JSP의 내장객체이기 때문에 사용할 수 없다.
- Default Page지정하기
- 현재 페이지 지정하기
- 현재 페이지에 해당되는 데이터를 DAO에 요청하기
  - map은 데이터저장 공간인데, key와 실제값을 저장할 수 있다.
- totalpage 값 받기
- 사용자가 요청한 mno데이터 받기

(2) HTML
- 데이터 출력부분
- 페이지 부분
- index (cateno)
  - cateno가 0이면 전체 보여주기



# 영화 상세보기 출력하기
## detail.jsp 생성
- 모양만 잡기

## main.jsp에 include하기
- mode가 8번일때, detail.jsp 출력되어야 함

## home.jsp에서 사진 클릭하면 mode8로 넘어가도록 설정하기
## detail.jsp에서 데이터 출력
- 사용자가 요청한 no값 받기

## movie-mapper.xml에서 SQL문장 작성하기
- parameteType에는 SQL문장에서 #|${}에 들어갈 데이터의 데이터 type을 작성한다.

## DAO 작성하기
- selectOne은 {}안의 매퍼 문장이 row한 개를 가져올 때 사용한다.
- selectList는 {}안의 매퍼 문장이 row 여러개를 가져올 때 사용한다.

## detail.jsp 받은 값 출력하기
(1) Java

(2) HTML
- 공백주는 법 : ```&lt;``` 작성하기
- 동영상 실행하는 태그 : ```<embed>``` , ```<iframe>```
- 줄거리 출력하는 태그 : ```<pre style="white-space: pre-wrap;background-color:white;border:none">```
