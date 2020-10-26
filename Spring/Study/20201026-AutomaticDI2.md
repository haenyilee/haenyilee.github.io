---
sort: 8
---

# AutomaticDI (2)

## 스프링에서 관리 대상 : 여러 곳에서 사용하는 기능
- DAO (Web)
- Manager (Web)
- Model
- Service

## 스프링 클래스 관리 순서
- 기능이 있는 모든 클래스는 스프링에 맡긴다.
- 필요시마다 스프링을 통해서 데이터를 얻어온다.
- 순서 : 생성 => DI(필요한 데이터 첨부) => 소멸

- **생성**
  - XML로 생성 : `<bean id="" class="">`
  - Annotaion으로 생성 : `<context:component-scan base-package="패키지명">`
    - 스프링에 의해 생성되는 클래스 : 클래스 위에 Annotation을 사용
    - 프로그래머가 직접 생성하는 클래스 : VO , interface
    
- **DI**
  - 일반데이터 : setter , constructor
  - 클래스 자체를 첨부 : @Autowired , 스프링에서 미리 생성되어 있는 클래스 객체의 주입 
    - @Autowired : 스프링에서 자동으로 첨부 , 원하는 클래스를 지정할 수 없음
    - @Resource(name="클래스id") : 스프링에 저장되어 있는 객체 중에서 특정 주소를 지정해서 가지고 온다.
    - 자동 주입은 반드시 스프링에서 메모리 할당을 해야 사용할 수 있다.
    

## 실습해보기
- xml없이 순수 자바로 코딩하기

### [VO] : MusicVO.java
- DTO , BEAN : 데이터를 넘겨서 전송할 목적을 가지고 있음
- 일반 데이터형으로 쓰임
- 스프링에서 메모리할당을 하지 않고 프로그래머가 필요시마다 메모리 할당을 함

### [DAO] : MusicDAO.java
- 스프링에서 관리함
- 스프링에서 MainClass쪽으로 결과를 전송하기 : app.xml
- `SqlSessionDaoSupport`
- BasicDataSource(오라클 정보 전달) => SqlSessionFactoryBean(커넥션 생성) => MusicDAO(PreparedStatement,ResultSet으로 결과값 전송)

- `@Repository` : 메모리 할당
  - 뒤에 아이디 작성하지 않으면 디폴트 id가 부여됨
  - ID : musicDAO
  - 맨 앞글자는 자동으로 소문자 적용

- 아래 app.xml에 작성했던 코드 자바로 대체하기

```xml
<bean id="dao" class="com.sist.di2.EmpDAO"
    p:sqlSessionFactory-ref="ssf"
/>
```

- setSqlSessionFactory

### [Spring Bean Configuration file] : app.xml
- `<context:component-scan base-package="패키지명">` : 클래스를 패키지 단위로 메모리 할당하기

### MyBasicDataSource.java
- app.xml에 작성하던 데이터 베이스 연결 정보를 아래 코딩을 대체하는 클래스

```xml
<bean id="ds" class="org.apache.commons.dbcp.BasicDataSource"
     p:driverClassName="oracle.jdbc.driver.OracleDriver"
     p:url="jdbc:oracle:thin:@211.238.142.181:1521:XE"
     p:username="hr"
     p:password="happy"
/>
```

- xml없이 순수 자바 코딩을 하기 위함임
- `extends BasicDataSource` 
  - setDriverClassName("");
  - setUrl("");
  - setUsername("");
  - setPassword("");

- `@Component` : 메모리할당
  - 뒤에 아이디 작성하지 않으면 디폴트 id가 부여됨
  - ID : myBasicDataSource

```note
#### 메모리할당하는 어노테이션?
- @Component : 일반클래스
- @Repository : 저장소(DataBase와 관련있기 때문에 DAO에서 주로 사용)
- @Controller : Model (return할때 사이트 주소, 즉 jsp 파일명이 넘겨줌)
- @RestController : Model (return할 때, 일반 문자열을 넘겨주기 떄문에 ajax,json에 사용됨)
- @Service : Manager,DAO가 여러개일때 묶어서 사용함
```
- 이 정보를 MySqlSessionFactoryBean.java로 넘겨줘야함


### MySqlSessionFactoryBean.java

- app.xml에 작성했던 아래 코드를 대체함

```xml
<bean id="ssf" class="org.mybatis.spring.SqlSessionFactoryBean"
     p:dataSource-ref="ds"
     p:configLocation="classpath:Config.xml"
/>
```

- `@Component` : 메모리 할당
  - 뒤에 아이디 작성하지 않으면 디폴트 id가 부여됨
  - ID : mySqlSessionFactoryBean
  - 맨 앞글자는 자동으로 소문자 적용

- MyBasicDataSource의 정보 넘겨 받기 : `p:dataSource-ref="ds"`
  - [Override/implements Method] - [setDataSource]
 
  ```java
  @Autowired
	public void setDataSource(DataSource dataSource) {
		// TODO Auto-generated method stub
		super.setDataSource(dataSource);
	}
  ```

```note
**@Autowired가 들어가는 위치**
- CONSTRUCTOR : 생성자
- FIELD : 멤버변수
- METHOD : 메소드
- PATAMETER : 매개변수
```

- Congig.xml 정보 넘겨 받기 : `p:configLocation="classpath:Config.xml"`

### [mapper] : music-mapper.xml
- `PreparedStatement` : SQL문장을 전송함
- `ResultSet`을 설정함 : 결과르 ㄹ전송함

### Config.xml 추가


### [View] : MainClass.java
- 클라이언트에서 데이터를 볼 수 있게 하는 클래스
- 웹으로 발전된다면, JSP가 담당하게 된다.
- 프론트엔드
- app.xml과 musicDAO를 통해 값 출력

## 실습해보기(2)
- MySqlSessionFactoryBean.java , MyBasicDataSource.java를 xml로 대체하기


### app2.xml
- 패키지 바깥에 위치해야한다.

```tip
**xml 공통으로 들어가는 부분**
1. 사용자 정의 클래스 등록하기 : 패키지 단위
--- 6장
2. AOP : 자동호출해야하는 반복코딩 제거(등록)
--- 7장
3. 데이터 소스 설정
4. 데이터 소스 트랜젝션 등록
5. SqlSessionFactoryBean 설정
--- 8장
6. WebSocket
7. 보안설정
```

- 사용자가 만든 클래스 메모리 할당하기 : `<context:component-scan base-package="com.haeni.di2"/>`
- 여러군데에서 di 가져다 쓸 수 있도록 설정하기

```xml
<bean id="ds" class="org.apache.commons.dbcp.BasicDataSource"
		p:driverClassName="oracle.jdbc.driver.OracleDriver"
		p:url="jdbc:oracle:thin:@211.238.142.183.1521:XE"
		p:username="hr"
		p:password="happy"
/>
<bean id="ssf" class="org.mybatis.spring.SqlSessionFactoryBean"
		p:dataSource-ref="ds"  <-- id="ds"인 DB 연결정보 가져옴 -->
		p:configLocation="classpath:Config.xml"
/>
```

### MainClass.java

- 메모리 할당하기 : `@Component`

- DAO를 자동 주입으로 받기

```java
@Autowired
	private MusicDAO dao;
```

- 스프링에 저장되어 있는 객체 가져오기 : `MainClass mc=(MainClass)app.getBean("mainClass");`
  - 주입받은 값 받으려면 메모리 할당을 new로 하면 안됨
  
- 스프링에 저장된 개체 list에 담아서 출력하기

```java
for(MusicVO vo:list)
{
	System.out.println("번호:"+vo.getMno());
	System.out.println("노래명:"+vo.getTitle());
	System.out.println("가수명:"+vo.getSinger());
	System.out.println("앨범:"+vo.getAlbum());
	System.out.println("=============================");
}

```



## 실습해보기 (3)

### MovieVO
- 데이터 컬럼 등록

### movie-mapper.xml
- 데이터 조회 쿼리문장 작성

### Config.xml
- typealias 및 mapper 위치 등록


### MovieDAO
- 메모리 할당
- ssf

### MainClass


