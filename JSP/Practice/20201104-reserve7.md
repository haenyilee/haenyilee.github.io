---
sort:
---

# reserve7

### model

```java
@RequestMapping("reserve/date.do")
  public String reserve_date(HttpServletRequest request)
  {
    // 선택한 날짜 받기
	  String strYear=request.getParameter("year");
	  String strMonth=request.getParameter("month");
	  String tno=request.getParameter("tno");
	  if(tno==null)
		  tno="1";
	  
    // 선택한 날짜 없으면 현재 날짜 전달
	  Date date=null;
		try {
			date = new Date();
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	  SimpleDateFormat sdf=new SimpleDateFormat("yyyy-M-d");
       // MM dd (X)  M d 01 ~ 09 10 11 12   09
       // 1 2 3 .... 10 11 12
      
	  String today=sdf.format(date);
	  StringTokenizer st=new StringTokenizer(today,"-");
	  
	  if(strYear==null)
	  {
		  strYear=st.nextToken();// yyyy
	  }
	  
	  if(strMonth==null)
	  {
		  strMonth=st.nextToken();
	  }
	  
	  int day=Integer.parseInt(st.nextToken());// 화면 
	  int year=Integer.parseInt(strYear);
	  int month=Integer.parseInt(strMonth);
	  
    // 캘린더 생성 후 년/월
	  Calendar cal=Calendar.getInstance();// Calendar클래스 생성 
	  cal.set(Calendar.YEAR,year);
	  cal.set(Calendar.MONTH,month-1);// month=>0~11
	  cal.set(Calendar.DATE,1);
	  
	  // 달 마지막 날의 요일을 구하기
	  int week=cal.get(Calendar.DAY_OF_WEEK);// 1~7 ==> week-1
	  int lastday=cal.getActualMaximum(Calendar.DATE);
	  String[] strWeek={"일","월","화","수","목","금","토"};
	  System.out.println("요일:"+strWeek[week-1]);
	  System.out.println("마지막날:"+lastday);
	  
	  // DB => 예약날짜 읽기
	  System.out.println("tno="+tno);
	  String rday=MovieDAO.theaterReserveData(Integer.parseInt(tno)); // rday = 영화 테이블 내의 날짜 받기
	  System.out.println("rday="+rday);
	  int[] days=new int[32];
	  StringTokenizer st2=new StringTokenizer(rday,",");
	  //int i=0;
	  while(st2.hasMoreTokens())
	  {
		  String d=st2.nextToken();
		  days[Integer.parseInt(d)]=Integer.parseInt(d);
	  }
	  
	  for(int k:days)
	  {
		  System.out.println("k="+k);
	  }
	  
    // jsp로 전송하기
	  request.setAttribute("rdays", days);
	  request.setAttribute("year", year);
	  request.setAttribute("month", month);
	  request.setAttribute("day", day);
	  request.setAttribute("week", week-1);
	  request.setAttribute("strWeek", strWeek);
	  request.setAttribute("lastday", lastday);
	  // 1일자의 요일 
	  return "../reserve/date.jsp";
  }
  ```

### date.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<!-- <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<style type="text/css">
.row {
   margin:0px auto;
   width:350pt;
}
</style> -->
<script type="text/javascript" src="http://code.jquery.com/jquery.js"></script>
<script type="text/javascript">

$(function(){
	$('.rdays_ok').hover(function(){
		$(this).css("cursor","pointer");
	},function(){
		$(this).css("cursor","");
	})
  
  // 선택된 년도를 #date_info(reserve.jsp)로 넘기기
	$('#year').change(function(){
		let year=$(this).val();
		$.ajax({
			type:'post',
			url:'../reserve/date.do', // 클라이언트가 HTTP 요청을 보낼 서버의 URL 주소
			data:{"year":year}, // HTTP 요청과 함께 서버로 보낼 데이터
			success:function(result) // HTTP 요청이 성공하면 요청한 데이터가 done() 메소드로 전달됨
			{
				$('#date_info').html(result);
			}
		})
	})
  
  // 선택된 월 #date_info(reserve.jsp)로 요청값 넘기기
	$('#month').change(function(){
		let month=$(this).val();
		$.ajax({
			type:'post',
			url:'../reserve/date.do', // 클라이언트가 HTTP 요청을 보낼 서버의 URL 주소
			data:{"month":month}, // HTTP 요청과 함께 서버로 보낼 데이터
			success:function(result)
			{
				$('#date_info').html(result); // HTTP 요청이 성공하면 요청한 데이터가 done() 메소드로 전달됨
			}
		})
	});
	
	// 예약일 클릭시  #time_info(reserve.jsp)로 요청값 넘기기
	$('.rdays_ok').click(function(){
		let year=$(this).attr("data-year");
		let month=$(this).attr("data-month");
		let day=$(this).text();
		let rday=year+"년도 "+month+"월 "+day+"일";
		$('#movie_reserve').text(rday);
		$('#day').val(rday);
    
		$.ajax({
			type:'post',
			url:'../reserve/time.do',
			data:{"day":day},
			success:function(result)
			{
				$('#time_info').html(result);
			}
		})
	})
});
</script>
</head>
<body>
      
      // 년,월 선택하기
      <h3 class="text-center">${year }년도 ${month }월</h3>
      <table class="table">
        <tr>
          <td>
            <select name="year" id="year">
             <c:forEach var="i" begin="2020" end="2030">
               <option ${i==year?"selected":"" }>${i }</option>
             </c:forEach>
            </select>년도&nbsp;
            
            <select name="month" id="month">
             <c:forEach var="i" begin="1" end="12">
               <option ${i==month?"selected":"" }>${i }</option>
             </c:forEach>
            </select>월
          </td>
        </tr>
      </table>
      
      // 달력 띄우기
      <table class="table table-striped">
        <tr class="danger" style="height:40px">
          <c:forEach var="str" items="${strWeek }" varStatus="s">
            <c:choose>
              <c:when test="${s.index==0 }">
                <c:set var="color" value="red"/>
              </c:when>
              <c:when test="${s.index==6 }">
                <c:set var="color" value="blue"/>
              </c:when>
              <c:otherwise>
                <c:set var="color" value="black"/>
              </c:otherwise>
            </c:choose>
            <th class="text-center"><font color="${color }">${str }</font></th>
          </c:forEach>
        </tr>
        <c:set var="week" value="${week }"/>
        <c:forEach var="i" begin="1" end="${lastday }">
           <c:choose>
              <c:when test="${week==0 }">
                <c:set var="color" value="red"/>
              </c:when>
              <c:when test="${week==6 }">
                <c:set var="color" value="blue"/>
              </c:when>
              <c:otherwise>
                <c:set var="color" value="black"/>
              </c:otherwise>
            </c:choose>
           <c:if test="${i==1 }">
             <tr style="height:40px">
             <c:forEach var="j" begin="1" end="${week }">
               <td>&nbsp;</td>
             </c:forEach>
           </c:if>
           
           <c:if test="${i==rdays[i] }">
             <c:set var="bg" value="text-center danger"/>
           </c:if>
           <c:if test="${i!=rdays[i]  }">
             <c:set var="bg" value="text-center"/>
           </c:if>
           
           <td class="${bg }"><font color="${color }">
            <c:if test="${i==rdays[i] }">
             <span class="rdays_ok" data-year=${year } data-month=${month }>${i }</span>
            </c:if>
            <c:if test="${i!=rdays[i]  }">
             ${i }
            </c:if>
           </font></td>
           <c:set var="week" value="${week+1 }"/>
           <c:if test="${week>6 }">
             <c:set var="week" value="0"/>
             </tr>
             <tr style="height:40px">
           </c:if>
        </c:forEach>
        </tr>
      </table>
  
</body>
</html>
```

###
