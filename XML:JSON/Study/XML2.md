#### 1. 테이블마다 VO만들어주기


#### 2. Config.xml파일 만들어주기

(1) DTD configuration 붙여넣기
```xml
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
```
(2) configuration
```xml
<configuration>
</configuration>
```

- properties?, 	 
  - properties에 파일을 만들어서 중요한 정보들을 저장함
  - 드라이버 이름, URL , UserName , Password(보안)
- settings?
  - 이미 실행된 SQL문을 저장해서 재사용할 수 있게 한다.(속도를 빠르게 해줌)
- typeAliases?
  - VO를 저장하는 역할(마이바티스 자체에서 값을 받아서 VO에 첨부한다)
- typeHandlers?
  - 오라클 데이터형과 자바 데이터형을 매칭시키는 역할
- environments?
  - 오라클 연결을 위한 정보를 세팅하는 역할
- mappers?
  - SQL문장을 저장한 파일이 등록
- objectFactory?, objectWrapperFactory?, reflectorFactory?, plugins?, databaseIdProvider?


(3) typeAlias

- VO를 등록하는 위치
- 마이바티스 클래스를 메모리 할당하고 set메소드를 데이터를 저장
- type에는 클래스명을 등록하기 
- type에는 클래스명 경로 너무 기니까 alias를 통해 별칭으로 간단하게 등록
```xml
<typeAliases>
    <typeAlias type="com.sist.dao.RecipeVO" alias="RecipeVO"/>
    <typeAlias type="com.sist.dao.ChefVO" alias="ChefVO"/>
</typeAliases>
```

(4) environments


(4) dataSource
- UNPOOLED
- POOLED
- JNDI : 

(5) property
- 미리 Connection을 만들기 위해서는 오라클 정보를 넘겨줘야 한다. 
- 이때, 필요한 디폴트 정보가 8개다. 

```xml
<property name="driver" value="oracle.jdbc.driver.OracleDriver/>
<property name="url" value="jdbc:oracle:thin:@211.238.142.181:1521:XE"/>
<property name="username" value="hr"/>
<property name="password" value="happy"/>
```

(6) mappers
- SQL문장의 위치를 등록
- 자체 폴더에 존재하면 resource사용한다.
  - 이때 중요한 것이 경로명이다.
- 원격으로 가져오게 되면 url을 사용하게 된다.
- 마이바티스나 스프링같은 경우는 경로명이 자동인식하는 위치가 있는데 이것이 바로 src이다. 
```xml
<mapper resource="com/sist/dao/recipe-mapper.xml"/>
<mapper resource="com/sist/dao/chef-mapper.xml"/>
```



#### 3. VO마다 mapper만들어주기
(1) DTD mapper 붙여넣기
```XML 
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
```

(2) mapper
- SQL을 저장하는 위치
- namespace는 XML에서 package에 해당하는 부분이다. 

- (cache-ref | cache | resultMap* | parameterMap* | sql* | insert* | update* | delete* | select*)+
  - ```*``` 가 붙어 있으면 0번 이상을 사용할 수 있는 것들임
  - ```|``` 가 붙어 있으면 필요한 부분만 사용이 가능하다는 뜻이다. 
  - ```resultMap``` : 컬럼명과 VO에 등록된 변수명이 다를 경우에 매칭시킬 수 있게 해준다. (JOIN시 사용됨)
  - ```parameterMap``` : PL/SQL (PROCEDURE)에서 사용됨
  - ```sql``` : 반복적으로 수행되는 부분들을 모아서 재사용을 할 때 사용한다. 
  - ```insert``` : 데이터를 추가할 때 사용
    - <insert>는 예외적으로 자동증가 번호 만들 때, 한 태그에 두 개의 문장을 사용할 수 있다.
    - SELECT와 INSERT만 사용 가능
  - ```update``` : 데이터를 수정할 떄 사용
  - ```delete``` : 데이터를 삭제할 때 사용
  - ```select``` : 데이터를 검색할 때 사용

- XML코드는 HTML과 다르게 대소문자를 구분하기 때문에 유의해서 써야한다.
- 마이바티스의 단점은 태그 하나에 여러개의 SQL문장을 사용할 수 없다는 점이다. 
  - 태그 하나당, 1개의 SQL문장을 작동한다. 

- SQL문장 뒤에 ```;```을 사용하면 오류가 난다.

- 데이터값을 설정할 때 ```?```를 사용하면 안되고, ```#{empno}```형식으로 사용해야 한다. 
  - ```#{empno}``` : 일반 데이터값을 받을 때 #을 사용한다.(=```getEmpno()```)
  - ```${empno}``` : 컬럼명이나 테이블명을 줄 때 $를 사용한다.
- 모든 태그들은 필수적으로 사용해야 하는 속성이 있다.
  - id : 중복이 없어야 한다. id를 가지고 SQL문장을 찾아야 하기 때문이다.
     - 키와 값으로 이루어진 Map형식으로 저장되기 때문에 키에 해당하는 id는 중복이 되어서는 안된다.
```xml
<mapper namespace="com.sist.dao.recipe-mapper">

</mapper>
```

(2) select
- ```id``` : 식별자
  - select문장이 여러개이기 때문에 특정 SQL문장을 찾아서 사용하기 위해서 필요한 데이터

- ```resultType```
  - SQL문장을 실행했을 때 결과값을 받는 클래스나 데이터 타입을 등록하는 것
  - ```selectList``` : VO를 받아서 ArrayList에 add()
  - ```selectOne``` : VO 1개만 받는 것

- ```parameterType``` : ```?```에 값을 첨부하는 역할을 함

  ```xml
  <select id="aaa" resultType="EmpVO" parameterType="int">
    SELECT * FROM emp
    WHERE empno=#{empno} AND ename=#{ename}     <!-- vo.getEmpno() vo.getEname()-->
  </select>
  ```

  - 만약에 VO에 없는 데이터값(여러개)이 들어간다면, <br>
```parameterType="java.util.Map"```을 통해서 값을 모아서 넣어줘야 한다.

    ```xml
    <select id="recipeListData" resultType="RecipeVO" parameterType="java.util.Map">
	SELECT no,title,poster,chef,num
	FROM (SELECT no,title,poster,chef,rownum as num
	FROM (SELECT no,title,poster,chef
	FROM recipe ORDER BY no DESC))
	WHERE num BEETWEEN #{start} AND #{end}
    </select>
    ```


#### 3. DAO 만들어주기
(1) XML을 읽어와서 데이터를 저장해두기
- 저장을 함과 동시에 셋팅됨 : getConnection() , disConnection()
- sqlSessionFactory : 마이바티스에서 지원하는 클래스
  - 기능 : XML 파싱

(2) id를 설정해주고 SQL문장이 실행할 수 있게 만들어 준다.
- 사용법
  1. XML을 이용해서 필요한 데이터를 전송하기
  2. Annotation을 이용해서 필요한 데이터를 전송하기

```
<프로그램을 짤 때, 고려해야 할 사항>
  1. 속도
  2. 쉽게 코딩
    - 표준화 : 마이바티스나 스프링은 표준화가 되어 있어서 분석하기가 쉽다.
    - 라이브러리를 이용하면 누구나 똑같이 코딩할 수 있다.
```

#### 4. 화면출력하기
(1) WebContent에 recipe폴더 만들기 <br>
(2) recipe.jsp파일 만들기 <br>
(3) bootstrap 받아오기 <br>
(4) 데이터 받아오기 <br>

1. 사용자의 요청 내역 받기 (페이지)
2. 요청을 받아서 데이터베이스에 있는 데이터를 받아오기
3. 받아온 데이터를 출력하기

#### 5. recipe-mapper에 페이지 나눠주는 SQL문장 저장


#### 6. RecipeDAO에서 사용자 요청시마다 페이지 나눠주는 기능 추가 
- 가져올 데이터 Record,Row가 한 개일때는 selectOne이지만 <br>
여러개일때는 selectList를 사용한다.
```java
private static int recipeTotalPage()
	{
		int total=0;
		SqlSession session =null;
		try {
			// 미리 생성해둔 Connection 객체 가져오기
			session=ssf.openSession();
			
			// 데이터 Record,Row가 한 개일때는 selectOne이지만 여러개일때는 selectList
			total=session.selectOne("recipeTotalPage");
			
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			if(session!=null)
				session.close();
		}
		return total;
	}
```

### 7. chef-mapper.xml 만들기
(1) packge 설정 => namespace 경로명을 지정하기
(2) select

#### 8. RecipeDAO.java에서 chefListData(Map map) 만들기
- map에는 start,end값이 들어감
- 데이터 얻기 완료하면, chef.jsp로 가서 화면으로 출력해야함
```java
// chef목록 얻어오기
	public List<ChefVO> chefListData(Map map)
	{
		// map에는 시작 위치, 끝위치가 들어감
		List<ChefVO> list=new ArrayList<ChefVO>();
		
		SqlSession session=null;
		try {
			// 연결할 수 있는 객체 얻어오기
			session = ssf.openSession();
			
			// SQL문장 보내고 결과값을 받아오기
			list=session.selectList("chefListData",map);
			
			// 더 간단히게 만드는 법 : 스프링 사용하기(열기, 닫기가 이미 만들어져 있음 : Annotation)
		} catch (Exception e) {
			e.printStackTrace();
		} finally {
			// 닫기 ==> 반환 (재사용이 가능하게 함)
			if(session!=null)
				session.close();
			
		}
		return list;
	}
```

#### 9. chef.jsp에서 화면 출력하기
```jsp
<%
	/* 
		1. 사용자가 보내준 데이터 받기
			- 사용자가 보내준 모든 데이터는 request안에 들어가 있다.
			- 사용자가 요청할때마다 톰캣이 request에 모아서 넘겨준다. 
			- 이 값을 가져오려면, getParameter()
		2. 사용자로부터 받은 데이터를 DAO에 넘겨준다
		3. DAO가 요청한 데이터를 보여준다
		4. DAO로부터 받은 데이터 화면에 출력한다.
			- MV방식은 HTML과 JAVA로 나눠서 처리하는 것이다.
			- 이때, HTML과 JAVA를 연결하는 것이 Controller이며 서블릿으로 만든다.
			- 이때, 자바 하나에서 데이터까지 접근하려니 복잡해서 JAVA(DAO)와 오라클(XML)을 나눠서 처리해준다. 
	*/
	
	// 사용자가 보내주는 값 => page
	String strPage=request.getParameter("page");

	// 첫 페이지
	if(strPage==null)
		strPage="1";
	
	// 현재 실행중인 페이지 번호
	int curpage=Integer.parseInt(strPage);
	
	// 현재 실행 중인 페이지의 데이터를 DAO에서 받아오기
	// DAO에 어디부터 어디까지 데이터를 보내달라고 요청
	int rowSize=20; // 한 페이지에 데이터를 20개씩 출력하기
	int start=(rowSize*curpage)-(rowSize-1);
	int end=rowSize*curpage;
	
	// start와 end를 Map으로 저장하기
	Map map=new HashMap();
	map.put("start",start);
	map.put("end",end);
	
	// DAO로 전송
	// 결과값을 받아오기
	List<ChefVO> list = RecipeDAO.chefListData(map);
	
	// 받은 데이터를 출력하기 : <body>에서
	
%>
``` 
#### 10. 쉐프리스트에서 쉐프이름 클릭하면 레시피 20개씩 나오도록 하기
(1) chef.jsp에서 링크 연결해주기
(2) 링크 연결된 페이지 만들기 : chef_list.jsp
(3) chef-mapper에서 데이터 가져오는 문장 작성하기
(4) DAO에서 데이터 가져오기
(5) DAO에서 얻어온 데이터를 chef_list.jsp에서 출력하기


### 자료구조
```
1. 데이터를 저장하는 방법
2. 데이터를 저장하고 쉽게 제어하는 것을 가능하게 하는 클래스의 집합이다.
3. 종류 : List , Set , Map
```
- List
  - 순서를 가지고 있다.
  - 데이터 중복이 가능하다.
  - 대표적인 클래스 : ArrayList , Vectot , LinkedList
- Set
  - 순서가 없다. 
  - 데이터가 중복이 없는 상태이다. 
  - 대표적인 클래스 : HashSet , TreeSet
- Map
  - 두 개를 동시에 저장이 가능한 자료구조
  - 키와 값으로 이루어져 있다. 
  - request, response
  - 대표적인 클래스: HashMap(hashtable의 단점을 보완) , hashtable
  - JSON이 대표적인 Map방식이다. (MongoDB도 JSON기반)
