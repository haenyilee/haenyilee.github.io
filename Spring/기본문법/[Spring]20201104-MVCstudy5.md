
# 20201104-MVCstudy (5) 게시판 만들기 : 자바로 INSERT, UPDATE,DELETE 구현하기

## 기초 셋팅
- pom.xml 변경
- web.xml 변경

## DB : 오라클 테이블 제작

```oracle
CREATE TABLE spring_board(
    no NUMBER PRIMARY KEY,
    name VARCHAR2(34) NOT NULL,
    subject VARCHAR2(1000) NOT NULL,
    content CLOB NOT NULL,
    regdate DATE DEFAULT SYSDATE,
    hit NUMBER DEFAULT 0
);
```

## DAO 패키지 제작

### BoardVO.java
- 컬럼에 맞춰 변수 선언
- 캡슐화 , 은닉화

```note
**캡슐화와 은닉화의 차이**
![](https://t1.daumcdn.net/cfile/tistory/99C27D345D1AE35F0A)
- 캡슐화란 특정 목적을 위해 데이터와 데이터를 다루는 메서드를 묶어서 추상화 하는 것이다.
    - 캡슐화라는 것은 메소드의 기능만 알고 그 기능을 사용할 뿐 실제로 메소드가 어떻게 움직이는지 굳이 알 필요 없는 것과 같다. 
    - 실제로 구현되는 부분을 외부에 드러나지 않도록 캡슐로 감싸 이용방법만을 알려주는것이다.
    - 데이터 구조와 데이터를 다루는 방법들을 결합 시켜 묶는 것. 다시 한번 말하자면 변수와 함수를 하나로 묶는것을 말한다.
    - 하지만 무작정 한대 묶으면 되는 것이 아니라 객체가 맡은 역할을 수행하기 위한 하나의 목적을 한데 묶는다고 생각해야한다.
    - 또한 데이터를 절대로 외부에서 직접 접근을 하면 안되고 오로지 함수를 통해서만 접근해야하는데 이를 가능하게 해주는 것이 바로 캡슐화이다.
      
- 은닉화라는 것은 캡슐화때문에 나오는 것인데, 클래스의 속성들을 private로 만들어 클래스 밖에서 건드리지 못하게 하는 것을 말한다.
    - 클래스를 사용함에 있어 속성들에 직접 접근하는 것은 데이터 무결성 오류 등에 치명적일 수 있기 때문에 이들을 접근하지 못하게 하는 것이다.
    - 대신 getter/setter라고 불리는 메소드를 통해서만 접근 가능케 하는 것이 은닉화이다.
    - 초기에는 번거로워 보이지만 들어와선 안될 데이터가 들어오는 것을 막을 수 있을 뿐 아니라 OOP(Object Oriented Programming)이라는 의미에 보다 가깝게 접근할 수 있다.
```

### BoardMapper.java
- Mapper namespace 와 ID를 연결할 Interface 를 두어서 interface를 호출하는 방법이다.
- `<select>`와 같이 xml에 작성하던 것을 `@Select`어노테이션으로 대체하여 자바에 코딩할 수 있다. 
- 기능별 쿼리문 작성
  - 1. 목록 출력
  - 2. 데이터 추가 : 자동증가번호 + 데이터 추가
    - 시퀀스를 사용하게 되면 자동증가번호를 따로 하지 않아도 된다.
  - 3. 상세보기
  - 4. 수정하기
  - 5. 삭제하기

```java
package com.haeni.dao;
import java.util.*;

import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.SelectKey;
import org.apache.ibatis.annotations.Update;
public interface BoardMapper {
  // 목록 출력
  @Select("SELECT no,subject,name,regdate,hit,num "
		 +"FROM (SELECT no,subject,name,regdate,hit, rownum as num "
		 +"FROM (SELECT no,subject,name,regdate,hit "
		 +"FROM spring_board ORDER BY 1 DESC)) "
		 +"WHERE num BETWEEN #{start} AND #{end}")
  public List<BoardVO> boardListData(Map map);
  @Select("SELECT CEIL(COUNT(*)/10.0) FROM spring_board")
  public int boardTotalPage();
  // 데이터 추가 
  @SelectKey(keyProperty="no",resultType=int.class,before=true,
		    statement="SELECT NVL(MAX(no)+1,1) as no FROM spring_board")
  @Insert("INSERT INTO spring_board(no,name,subject,content,pwd,filename,filesize,filecount) "
		 +"VALUES(#{no},#{name},#{subject},#{content},#{pwd},#{filename},#{filesize},#{filecount})")
  public void boardInsert(BoardVO vo);
  // 상세보기 
  @Update("UPDATE spring_board SET "
		 +"hit=hit+1 "
		 +"WHERE no=#{no}")
  public void boardHitIncrement(int no);
  @Select("SELECT no,name,subject,content,regdate,hit,filename,filesize,filecount "
		 +"FROM spring_board "
		 +"WHERE no=#{no}")
  public BoardVO boardDeteilData(int no);
  // 수정 
  @Select("SELECT pwd FROM spring_board "
		 +"WHERE no=#{no}")
  public String boardGetPassword(int no);
  
  @Update("UPDATE spring_board SET "
		 +"name=#{name},subject=#{subject},content=#{content},"
		 +"filename=#{filename},filesize=#{filesize},filecount=#{filecount} "
		 +"WHERE no=#{no}")
  public void boardUpdate(BoardVO vo);
  // 삭제  
  @Delete("DELETE FROM spring_board "
		 +"WHERE no=#{no}")
  public void boardDelete(int no);
  // 삭제 - 1
  @Select("SELECT filename,filecount,filesize "
		 +"FROM spring_board "
		 +"WHERE no=#{no}")
  public BoardVO boardFileInfoData(int no);
}
```


### BoardDAO.java
- `@Repository` : 메모리 할당
- `@Autowired` : 
- mapper 등록
- 기능별 할당된 매퍼 주소 요청?
  - 1. 목록 출력
  - 2. 데이터 추가 : 자동증가번호 + 데이터 추가
    - 시퀀스를 사용하게 되면 자동증가번호를 따로 하지 않아도 된다.
  - 3. 상세보기
  - 4. 수정하기
  - 5. 삭제하기

```java
package com.haeni.dao;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;
import java.util.*;
@Repository
public class BoardDAO {
	@Autowired
    private BoardMapper mapper;
	// 목록 읽기
	public List<BoardVO> boardListData(Map map)
	{
		return mapper.boardListData(map);
	}
	// 총페이지 읽기
	public int boardTotalPage()
	{
		return mapper.boardTotalPage();
	}
	// 상세보기 
	public BoardVO boardDetailData(int no)
	{
		mapper.boardHitIncrement(no);
		return mapper.boardDeteilData(no);
	}
	// 추가 
	public void boardInsert(BoardVO vo)
	{
		mapper.boardInsert(vo);
	}
	// 수정 
	public BoardVO boardUpdateData(int no)
	{
		return mapper.boardDeteilData(no);
	}
	// 실제 수정 
	public boolean boardUpdate(BoardVO vo)
	{
		boolean bCheck=false;
		String db_pwd=mapper.boardGetPassword(vo.getNo());
		if(db_pwd.equals(vo.getPwd()))
		{
			bCheck=true;
			mapper.boardUpdate(vo);
		}
		else
		{
			bCheck=false;
		}
		return bCheck;
	}
	// 삭제
	public boolean boardDelete(int no,String pwd)
	{
		boolean bCheck=false;
		String db_pwd=mapper.boardGetPassword(no);
		if(db_pwd.equals(pwd))
		{
			bCheck=true;
			mapper.boardDelete(no);
		}
		else
		{
			bCheck=false;
		}
		return bCheck;
	}
	public BoardVO boardFileInfoData(int no)
	{
		return mapper.boardFileInfoData(no);
	}
	
}
```

### [config] application-context.xml
- 패키지 정보등록 : `<context:component-scan base-package="com.sist.*"/>`
- 한글변환코드 : `<mvc:annotation-driven/>`
  - RestController에서 데이터 전송 시 한글이 깨진다.
- jsp 찾는 viewResolver 등록


### [config] application-datasource.xml
- 데이터 베이스 관련 정보만 등록
  - 1. DataSource : 커넥션 관련 정보
  - 2. 마이바티스에 값 전송
  - 3. 인터페이스 구현(공통모듈)

- tx : 트랜젝션
  - 댓글 올리면 여러개가 동시에 수행되어야 하기 때문에 트랜젝션이 필요하다.
  - insert, update, delete에서만 사용함
  - 예를 들어 제품을 입고시키면 재고도 동시에 처리해줘야 하기 때문에 트랜젝션이 사용된다.
  

### BoardController.java
- `@Controller`: 메모리 할당
- 스크립트를 보내는 것이 불가능하다.
- 파일 변경만 가능하다.
  
- 1. 스프링으로부터 필요한 클래스 객체를 받아 두기 : `@Autowired private BoardDAO dao;`
  - 스프링에서 생성된 객체 주소를 받을 경우에 지역변수는 사용할 수 없다.
  - FIELD : 멤버변수

```NOTR
**@Autowired 위치**
- FIELD
- CONSTRUCTOR
- ....
```

- 2. `@RequestMapping`
  - foward일 때는 `"board/list"`
  - redirect일 때는 `"redirect:.do"`
  

### BoardRestController.java
- `@RestController`: 메모리 할당
- 두 개 이상이 한번에 처리될 때 사용한다.
- 필요한 자바스크립트를 함께 보낼 수 있다.


