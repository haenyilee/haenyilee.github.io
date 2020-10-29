---
sort:
---

# Model2 방식으로 웹사이트 만들기 (2) 마이페이지

## Mypage에 출력할 데이터 가져오기

### [VO]
- MovieVO 클래스를 변수로 추가하기
  - JOIN을 통해 값을 얻어와야 하기 때문이다.
  
```java
private MovieVO mvo=new MovieVO();
	
	public MovieVO getMvo() {
		return mvo;
	}
	public void setMvo(MovieVO mvo) {
		this.mvo = mvo;
	}
```
  
  
  
### [mapper]
- `resultMap` : `getMvo()`는 있지만, MovieVO 각 객체들의 get메소드는 없기 떄문에 이를 컬럼명과 매칭해줄때 필요한 기능이다.
  - `vo.getMvo().setPoster(rs.getString("psoter"));`의 자바 코딩과 동일한 기능이다.
  
```xml
<resultMap type="com.sist.vo.ReserveVO" id="reserveList">
  	<result property="no" column="no"/>
  	<result property="id" column="id"/>
  	<result property="theater" column="theater"/>
  	<result property="time" column="time"/>
  	<result property="inwon" column="inwon"/>
  	<result property="price" column="price"/>
  	<result property="isreserve" column="isreserve"/>
  	<result property="mvo.title" column="title"/>
  	<result property="mvo.poster" column="poster"/>
</resultMap>
```

- JOIN 쿼리문장 작성할 때, 두 컬럼명이 틀리면 `테이블.컬럼명` 형식으로 작성하지 않아도 됨

```oracle
  <select id="mypageReserveListData" resultMap="reserveList" parameterType="string">
	SELECT reservo.no,title,poster,theater,time,inwon,price,isreserve
	FROM reserve,movie_info
	WHERE mno=movie_info.no AND id=#{id}
  </select>
  <select id="adminpageReserveListData" resultMap="reserveList">
	SELECT no,id,title,poster,theater,time,inwon,price,isreserve
	FROM reserve,movie_info
	WHERE mno=no
  </select>
```

### [DAO]
- mypageReserveListData , adminpageReserveListData 쿼리 문장 실행하기

### [Model]
- Ajax가 아닌 include방식임

### [View]

#### mypage.jsp
- `<c:if test="">` 태그를 통해서 예매 완료일 때와 예매 완료되지 않았을 때 버튼을 달리하기


#### mypage.jsp
- `<c:if test="">` 태그를 통해서 승인 완료일 때와 승인이 완료되지 않았을 때 버튼을 달리하기


## 승인대기 상태 결제건 승인완료하기

### adminpage.jsp

- 승인대기 버튼 경로 : `..reserve/admin_ok.do?no=${vo.no}`
  - 이 버튼을 누르면 isreserve의 상태가 `n`에서 `y`로 변경되어야 함
  

### [mapper]
- 승인 대기 > 승인 완료로 변경하는 쿼리문장 작성하기

```xml
<update id="reserveOk" parameterType="int">
  	UPDATE reserve SET
  	isreserve='y'
  	WHERE no=#{no}
</update>
```


### [DAO]
- 승인 대기 > 승인 완료로 변경하는 쿼리문장 처리하기
- UPDATE 처리 후, AutoCommit 실행될 수 있도록 `openSession(true)` 값 주기

```tip
**사용자가 요청할 수 있는 위치**
1. `<a href="사용자 요청 페이지">`
2. `<form action="사용자 요청 페이지">`
3. ajax에서의 url:'사용자 요청 페이지'
```

### reserveModel.java
- `@RequestMapping("reserve/admin_ok.do")` 
  - `reserve/admin_ok.do` 요청이 들어오면 이를 처리하는 메소드 찾는 것이 어노테이션의 역할이다.
  - 상세 처리 순서 : 사용자 요청(`*.do`) → DispatcherServlet(Controller) → Model(RequestMapping)
- 사용자가 클릭한 예약번호 받아서 쿼리문장 실행한 뒤, adminpage로 redirect하기


## 승인 대기 상태 한번에 처리하기

### mapper
- 승인 미완료건들은 페이지 목록에 뜨지 않게 쿼리 문장 수정하기

```jsp
  <select id="adminpageReserveListData" resultMap="reserveList">
	SELECT no,id,title,poster,theater,time,inwon,price,isreserve
	FROM reserve,movie_info
	WHERE mno=movie_info.no AND isreserve='n'
  </select>
```

### adminpage.jsp
- 승인 완료된 건들만 체크박스 만들기

```jsp
<c:if test="${vo.isreserve=='n' }">
	<input type="checkbox" value="${vo.no }" class="cb" name=cb>
</c:if>
```

- form 태그로 테이블 묶기

- 전체 승인하는 버튼 만들기
  - 각각 체크 후, 이 버튼을 만들면 전체 승인 완료로 변함
  
- 체크가 됐는지 유효성 검증하는 제이쿼리문 작성하기(자바 스크립트)
  - 스프링에서는 리액트나 뷰로 대체함
  - 아이폰과 안드로이드를 동시에 다뤄야 하기 때문이다.(native라는 라이브러리를 쓰면 가능하기 때문이다.)
  - 변수는 let을 사용하는 것이 편하다(var은 구버전임)
  - 체크박스만 가져오려면? : `$('input[type=checkbox]')`
  
  ```note
  **태그명 가지고오기(SELECTOR)**
  1. $('태그명')
  2. $('#id')
  3. $('.class')
  ```
  
  ```note
  **제어하기**
  1. text() : 태그 사이에 태그가 또 있어도 값만 가져온다
  2. attr(속성명)
  3. val() : input이나 selector의 value를 가져올 때 사용한다.
  4. html()
  5. append() : 태그 사이에 태그를 여러개 첨부하고 싶을 때 사용한다.
  6. hide() / show() : 감추기/보여주기 (더보기 기능에서 주로 사용)
  7. contains() : 데이터가 쭉 있는데 내가 원하는 데이터만 찾을 때 사용한다.
  8. parent() : 상위 태그 클릭
  9. child() : 하위 태그 클릭
  ```
  
  - 체크 박스 선택 됐으면 form태그의 action에 등록된 요청 실행하기
```tip
**가장 많이 등장하는 이벤트**
1. click 
2. change : select태그에 사용됨
3. hover
```

- 전체 선택 하려면? : `.cb:clicked`

### ReserveModel.java
- 체크된 예약번호 배열로 받아서 하나씩 reserveOk 실행시키기

```java
  @RequestMapping("/reserve/reserve_all_ok.do")
  public String reserve_all_ok(HttpServletRequest request)
  {
	  // 데이터 받기
	  String[] nos=request.getParameterValues("cb");
	  for(String n:nos)
	  {
		  MovieDAO.reserveOk(Integer.parseInt(n));
	  }
	  return "redirect:../reserve/adminpage.do";
  }
```

## 영화 검색 기능만들기

### movie.jsp
- 검색 창 만들기 

  ```jsp
     <table class="table">
   	<tr>
   		<td>
   		<input type=text id="keyword" size=15 placeholder="검색">
   		</td>
   	</tr>
   </table>
  ```

- 검색 관련 제이쿼리 문장 작성하기
  - 영화 검색 함수 만들기 : `$('#keyword').keyup(function(){}}`
    - `keyup` : 글자를 다 썼을 때 (키보드 뗐을 때)
    - `keydown` : 글자를 쓰고 있을 때 (키보드 누르고 있을 때)
  - 검색어 입력값 읽어오기 : `let k=$('#keyword').val();`
  - 영화 목록 전체 감추기 : `$('#movie-table > tbody > tr').hide();`
    - `>` : 하위 태그 
    - `tbody`
  -  : `let temp=$('#movie-table > tbody > tr > td:nth-child(2n+2):contains("'+k+'")');`
    - 2번째 컬럼 데이터(title부분) 전체 읽어오기 : `$('#movie-table > tbody > tr > td:nth-child(2n+2)`
    - 검색한 키워드 포함되는 것 찾기 : `:contains("'+k+'")`
  - 검색한 제목의 상위태그(전제 데이터) 보여주기 : `$(temp).parent().show();`
  
 
