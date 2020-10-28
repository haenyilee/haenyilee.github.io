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
