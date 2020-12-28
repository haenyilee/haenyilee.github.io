---
sort:
---

# Model2 방식으로 웹사이트 만들기 (1) 예매하기
- 깃허브 : https://github.com/haenyilee/JSP-Study/tree/master/MovieMiddleProject

## 달력 만들기

### [reserveModel.java]
- 오늘 날짜 구하는 것부터 시작해야함
  - yyyy-MM-dd가 아닌 yyyy-M-d를 써야함
  - 8진법을 넘어서면 오류가 나기 때문이다?...
- 년/월/일 stringTokenizer로 잘라내기
- 디폴트 날짜 설정하기
  - 달력에서 년/월은 설정되어 있어야 하지만, 일은 1일~마지막일까지 전부 다 출력해야 하니까 null값일 수가 없다?
- 요일 구하기
  - 매달 1일의 요일을 구해야 달력을 출력할 수 있음
    - week는 1~7까지로 이루어져 있어서 0번부터 시작하는 자바에선 1을 빼야함
    - month는 0~11까지로 되어있음
  - 매달 마지막 날짜 구하기
  
- jsp로 전송시키기

```java
@RequestMapping("reserve/date.do")
  public String reserve_date(HttpServletRequest request)
  {
	 String strYear=request.getParameter("year");
	 String strMonth=request.getParameter("month");
	 Date date = new Date();
	 SimpleDateFormat sdf= new SimpleDateFormat("yyyy-M-d");
	 String today=sdf.format(date);
	 StringTokenizer st= new StringTokenizer(today,"-");
	 
	 if(strYear==null)
	 {
		 strYear=st.nextToken();
		 
	 }
	 if(strMonth==null)
	 {
		 strMonth=st.nextToken();
		 
	 }
	 
	 // 오늘 날짜
	 int day=Integer.parseInt(st.nextToken());
	 int year=Integer.parseInt(strYear);
	 int month=Integer.parseInt(strMonth);
	 
	 // 1일자의 요일 구하기
	 Calendar cal=Calendar.getInstance();
	 cal.set(Calendar.YEAR,year);
	 cal.set(Calendar.MONTH,month-1); // month는 0~11까지로 되어있음
	 cal.set(Calendar.DATE,1);
	 
	 int week=cal.get(Calendar.DAY_OF_WEEK); // week는 1~7까지로 이루어져 있음
	 int lastday=cal.getActualMaximum(Calendar.DAY_OF_MONTH);
	 
	 String[] strWeek= {"일","월","화","수","목","금","토"};
	 
	 // 디버깅
	 System.out.println("요일:"+week);
	 System.out.println("마지막날:"+lastday);
	 
	 request.setAttribute("year", year);
	 request.setAttribute("month", month);
	 request.setAttribute("day", day);
	 request.setAttribute("week", week-1);
	 request.setAttribute("strWeek", strWeek);
	 request.setAttribute("lastday", lastday);
	 
	 
	 return "../reserve/date.jsp";
  }
```


### [date.jsp]
- 달력 모양 잡기
- 주말은 color 변경하기
  - 배열의 인덱스 번호를 가져올 수 있으므로, 요일 배열 중에서 인덱스가 0,6인 것들 색상 변경하기
- 1일이라면 그 전 날짜들 들어갈 공백 띄우기 (`<tr>`)
- 요일이 6보다 커지면, 다음주로 넘기기
  - 다음주로 넘어가면 요일은 0으로 reset된다.
- 오늘 날짜에 색상 표시하기
- 날짜직접 선택하는 select 추가하기

### [main.jsp]
- Ajax 



## 예매 가능 영화 출력

### [movie-mapper.xml]
- LIKE 뒤에 변수가 올때는 `'%||#{ss }||%'`형식으로 작성하면 된다. 

### [MovieDAO.java]

### [MovieModel.java]

- main.jsp로 반환해서 include하는 것이 아니라, movie.jsp로 이동해서 AJAX가 화면을 출력해주는 방식이다. 
  - 기존화면은 유지되고, 추가되는 부부만 삽입되는 형식이다.
  - append를 주게되면 클릭할때마다 추가된다. 
  
  ```note
  #### 화면 삽입하는 방법
  - switch문을 통해서 text를 uri에 삽입하기
  - main에서 include하기
  - ajax로 특정 부분만 추가하기
  ```
  
  
  
  
## 극장 출력하기

### [Model]
- 곧장 보내고 실행결과 바로 받아오기 때문에 화면이 깜빡이지 않음

- jsp에서 서버로 값 보냄
- 서버에서 결과값 보냄
- 결과값을 jsp로 보냄

- Ajax코딩
- Asynchronous Javascript And XML
  - 동시에 여러가지를 처리할 수 있지만 속도가 느림
  - HTML을 읽어오기 때문에 읽어올 데이터가 많아서 속도가 느리다.
- 브라우저에 요청
- XMLHttpRequest req : 브라우저에 있는 객체 
1. 서버 연결 : req.open("POST" , "URL",true/false)
	- true : 비동기방식
	- false : 동기화방식
2. 

### VO

### mapper

### model

### [View] theater.jsp

## 영화관/영화시간 정보

### DB제작

```oracle
DESC theater_info;
ALTER TABLE theater_info ADD rday VARCHAR2(300);
CREATE TABLE date_info(
	rday NUMBER PRIMARY KEY,
	rtime VARCHAR2(200) 
);
CREATE TABLE time_info(
	tno NUMBER PRIMARY KEY,
	time VARCHAR2(30)
);

INSERT INTO time_info VALUES(1,'08:00');
INSERT INTO time_info VALUES(2,'09:00');
INSERT INTO time_info VALUES(3,'10:00');
INSERT INTO time_info VALUES(4,'11:00');
INSERT INTO time_info VALUES(5,'12:00');
INSERT INTO time_info VALUES(6,'13:00');
INSERT INTO time_info VALUES(7,'14:00');
INSERT INTO time_info VALUES(8,'15:00');
INSERT INTO time_info VALUES(9,'16:00');
INSERT INTO time_info VALUES(10,'17:00');
INSERT INTO time_info VALUES(11,'18:00');
INSERT INTO time_info VALUES(12,'19:00');
INSERT INTO time_info VALUES(13,'20:00');
INSERT INTO time_info VALUES(14,'21:00');
INSERT INTO time_info VALUES(15,'22:00');
INSERT INTO time_info VALUES(16,'23:00');
INSERT INTO time_info VALUES(17,'24:00');
COMMIT;
```


### [temp - MovieDAO]
- 가상으로 데이터 넣기

### [reserve - theater.jsp]
- 극장 정보 하나를 넘겨줘야 달력을 띄울 수 있음



## 달력 ajax로 출력하기


### movie-mapper.xml
-  `id="theaterReserveData"` 쿼리문장 작성하기

### movieDAO
- `theaterReserveData`의 결과값 반환하기

### [reserveModel.java]
- request값 받기
- db에서 예약날짜 읽어오기

### [View]
- 가능한 날짜만 달력에 표시하기
- 표시된 날짜만 예약 가능하게 하기

### [date.jsp]
- 예약일 클릭했을 때, 값 받아서 출력할 수 있도록 자바스크립트문 작성하기

### [reserve.jsp]
- 선택한 예매일 출력하기


## 시간대 선택하기

### temp - DAO
- 랜덤으로 데이터값 집어넣기

### temp - MainClass

- 5~14 중에서 선택?
- `Array.sort(com);`
  - desc 만 있음, asc는 불가
  
  ### date.jsp
  - 시간 출력하기
  - ajax
    - 누구한테 보낼지 url 설정하기
    - 보내야하는 것 (Data): 날짜 , date
    - result : text , html , xml , json
    - 현재 만든 dao가 
    
### [Model]
- reseve/time.do 요청 들어오면 결과값 전송해주기
    
    
### mapper
- 날짜에 해당하는 time데이터 찾는 쿼리문장 작성하기
- 

### [DAO]
- 날짜 클릭하면 날짜안에 시간들이 있으니까 그 데이터 가져오기
- 번호 데이터를 가져와서 시간대 매칭시키기?

### [Model]
- db데이터 베이스 결과값 가져오기
- 시간이 여러개 담겨있으니까 arraylist에 담아서 `,`로 짤라서 저장하기

### [time.jsp]
- 받은 값 출력하기>?
- 예약 정보로 넘아갈 애들임


### [inwon.jsp]
- 성인 , 청소년 명수 클릭 버튼 만들기
- 버튼은 	`<:forEach>`로 출력하기 
  -  0명 ~8명까지 버튼임


### [time.jsp]
- ajax작성 

### reserve.jsp
- 시간 정보, 인원 값 넣기

## 인원 출력하기

### [mapper]

### inwon.jsp
- 여기서 처리하고 결과는 ajax를 통해 reserve.java로 전송해서 보여줘야함
- 이벤트 처리 등은 각자 jsp에서 해야함, 출력만 한 곳에서!
 
### reserve.jsp
- 인원, 금액 정보 값 넣기

## 예매 완료 처리하기

### db제작

- 영화 번호를 저장해둠
- 영화 테이블과 JOIN을 걸어서 값을 가져올 수 있음
- `isreserve`는 예매 완료 여부 확인하는 컬럼임 (결과 : y/n)

```oracle
CREATE TABLE reserve(
    no NUMBER PRIMARY KEY,
    id VARCHAR2(20) NOT NULL,
    mno NUMBER,
    theater VARCHAR2(100),
    time VARCHAR2(20),
    inwon VARCHAR2(20),
    price VARCHAR2(10),
    isreserve CHAR(1) DEFAULT 'n',
    regdate DATE DEFAULT SYSDATE
);

ALTER TABLE reserve MODIFY time VARCHAT2(200);
```

### 요소 클릭할떄마다 hidden 방식으로 값 채우기

- reserve.jsp
  - display : none으로 form태그 hidden방식으로 값 받아서 감춰두기

- movie.jsp
- theater.jsp
- date.jsp
- time.jsp


## 예매 완료?

### movie-mapper.xml
- insert쿼리문장 작성하기
- `<selectKey>`
  - insert할 때 사용가능한 기능이다.
  - `keyProperty` : vo의 변수와 쿼리문장의 컬럼명이 일치하면 result의 결과를 keyProperty에 넣어준다.
  
### DAO
- Insert문장은 COMMIT을 꼭 해야함을 유의해야 함

### Model
- DAO에서 실행 후 반환된 결과값 담아서 이동하기
