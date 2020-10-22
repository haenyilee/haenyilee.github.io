---
sort:
---

# [JSP-Model2] 영화 예매하기

### 코드 : MovieMiddelProject

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
  
  
  

