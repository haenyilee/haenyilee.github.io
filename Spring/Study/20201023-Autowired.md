---
sort: 7
---

# 의존 자동 주입 - Autowired

---
#### 참고 
- 책 : 48-58page,75-88page
- 코드 : ()[]
---


```tip
- 라이브러리는 작동되는 과정을 볼 수 없기 때문에 오류를 잡기 힘들다.
- output창을 보면서 혼자 오류볼 수 있는 힘을 키워야 한다. 
  ```


## Spring을 사용하는 방법
1. XML만 이용하는 방식
2. Annotaion만 이용하는 방식
3. XML+Annotation을 사용하는 방식 (실무에서 가장 많이 사용하는 방식)
4. 순수 자바만 사용하는 방식 (5버전에 사용하는 차세대 방식)


## 
- 스프링은 디폴트가 싱글턴 패턴이다.


## @Autowired 애노테이션을 이용한 의존 자동 주입
## @Qualifier 애노테이션을 이용한 의존 객체 선택
## 상위/하위 타입 관계와 자동 주입
## @Autowired 애노테이션의 필수 여부
## 자동 주입과 명시적 의존 주입 간의 관계

## 실습해보기(1) : 순수 XML방식
- 지니 뮤직차트 데이터를 활용한다.


### 1. 기본 셋팅
### [MovieVO] : 스프링에서 관리
### [MovieDAO] : 스프링에서 관리X

- extends SqlSessionDaoSupport
  - 마이바티스에서 지원하는 스프링과 연동하는 API를 상속받는다.
  - org.mybatis.spring.SqlSessionFactoryBean
  - org.mybatis.spring.SqlSessionTemplate
  
- getSqlSession() 
  - SqlSession session=ssf.openSession();
  
- selectList("musicAllData") :
  - list=session.selectList("");
	- session.close();  : 스프링이 자동처리
	- return list;
  
  
### [Confid.xml] 
- src/java에 위치함


### [app.xml]
- 데이터 베이스 정보 생성하기 : BasicDataSource

- MyBatis에 전송하기 : getConnection, disConnection

- DAO전송하기 : 활용하기

### 2. Main에서 출력하기

- app.xml 활용하려면 ApplicationContext

- 스프링에서 객체 생성한 DAO 읽어오기


### 
- @Component : 객체 생성
- @Autowired : 객체 메모리 할당

- app.xml  에 등록된 base-package에서 아래 어노테이션이 존재하면 메모리 할당해준다.
  - 메모리 할당 : @Component, @Repository, @Service, @Controller, @RestController, @ControllerAdvice, @Configuration 
- 메모리 주입 " @Required, @Autowired, @PostConstruct, @PreDestroy, @Resource, 

##
- @Autowired
- @Qualified("oracle")
- @Resource(name="mySQL")
- app.getBean("mySQL") , 객체 instance of연산자 클래명 (값이 같을 때 객체가 클래스와 같냐? 같으면 true/틀리면 false) loop가 돌아감

```java
String s // String은 Object를 상속받음
StringBuffer sb // StringBuffer도 Object를 상속받음
Object obj

if(s instanceof String)  // true
if(s instanceof Object)  //true
if(sb instanceof StringBuffer) // true
if(obj instanceof StringBuffer) // false
if(sb instanceof String) // error : String과 StringBuffer은 남자와 여자처럼 관련없기 때문이다.
```




## @Autowired
- 스프링에 관리할 대상의 클래스 객체를 저장한다.
- 저장된 클래스의 주소갑을 스프링에 자동으로 주입(전송)한다.



## Annotaton
- Type : 찾는 것 위에 있음

- spring에서 @Component아래 있는 클래스는 메모리 할당을 한다.
- 어떻게 어노테이션 있는 클래스와 없는 클래스를 구분해서 메모리 할당을 할까?


## interface
- 같은 인터페이스를 두 클래스가 받으면 오류가 난다. 
- 이 것을 제어하는 방법이 '@Qualifier'이다. 

```java
interface I
class A implements I
class B implements I

@Autowired
I i =new A();
I i =new B();
```

### 각 클래스에 @Component붙었는지 확인해서 메모리 할당하기
- String안에 있는 클래스를 다 찾아서 list로 모은다.
- for문을 이용해서 클래스명을 찾아서 메모리 할당을 한다. (Class.forName)
- if문, `isAnnotationPresent`을 통해 @Component가 붙은 클래스인지 확인한다.







## 스프링이  지원하는 것
- DI , AOP
- MVC (라이브러리)
- 공통 모듈을 싱글턴으로 제작해서 반복을 제거하고 자동으로 메모리를 최적화하게 해준다.
- 메모리의 생성부터 소멸까지 자체 담당해준다. 
  - 객체 생명주기 : 객체가 생성되고 소멸되는 주기
    - 생성
    - constructorDI 
    - setterDI 
    - init-method 
    - 프로그래머가 조립 
    - destroy-method
    - 소멸
  - 필요한 데이터가 있을 때만 DI

