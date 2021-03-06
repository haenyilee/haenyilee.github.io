# 20201022-DIstudy (4)

### 참고
- [초보웹개발자를 위한 스프링5 프로그래밍 입문] 90~102page

## 스프링의 서버와 클라이언트
![](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F247D803A5834FC2C35)

- 서버에서 무슨일을 하는지 클라이언트가 모르게 해야 한다. 
- 사용에 영향을 미치면 안되기 때문이다.

- [Main메소드]가 있는 영역이 [Client부분]이다. 
  - 클래스를 관리하는 Container
 
- [ApplicationContext]이 [Server부분]이다.
  - 메시지, 프로필 환경변수 등을 처리할 수 있는 기능을 추가로 정의한다.

  - 여러개가 필요할 때는 ProtoType Pattern , 한 개만 필요할 때는 Singleton Pattern을 사용한다.
    - XE는 500개 정도 접근이 가능하다.
    - 커넥션 1000만 개 이상 저장할 수 있는 컴퓨터는 없다. 
    - 그래서 커넥션 객체 생성을 조절할 수 있어야 하는데, 한 사람당 서버 하나만 사용하게 하는 것이 싱글턴 패턴이다. 
    - DBCP 
    ![](https://t1.daumcdn.net/cfile/tistory/21373937580AEF9B37)
  
## Container란?
- 기본적으로 Container는 여러개의 클래스를 모아서 관리를 하는 영역을 지칭한다.
- 클래스의 객체를 쌓아두는 공간이라고 볼 수 있다. 

- ApplicationContext : 스프링 4버전에서 주로 사용한다.
  - XML을 파싱하는 기능을 가지고 있다.
  - 빈 객체의 생성, 초기화, 보관, 제거 등을 관리하고 있어서 **컨테이너**라고 부른다.
  
- AnnotaionConfigApplicationContext : 스프링 5버전에서 자주 사용한다.
  - Annotaion 관련 기능
  - 자바 설정에서 정보를 읽어와 빈 객체를 생성하고 관리한다.
  - ApplicationContext 인터페이스를 알맞게 구현한 클래스 중 하나이다.
  - `ÀnnotaionConfigApplicationContext`: 자바 어노테이션을 이용하여 클래스로부터 객체 설정 정보를 가져온다.
  - `GenericXmlApplicationContext` : XML로부터 객체 설정 정보를 가져온다.

## 스프링 DI (Dependency Injection : 의존 주입)

### 의존이란?
- 한 클래스가 다른 클래스의 메서드를 실행할 때, **의존**한다고 표현한다.
- 의존관계에 있는 메서드는 변경에 의해 영향을 받는다. 


### DI를 통한 의존 처리
- 객체를 직접 생성하는 대신 의존 객체를 전달받는 방식이다.
- 의존 객체를 직접 생성하는 방식 : new연산자를 이용하여 객체를 생성하는 것이다. 
- DI를 통해 객체를 주입하는 이유는 **변경의 유연함**때문이다.


### 스프링의 기능
- 스프링은 DI를 지원하는 조립기라고 볼 수 있다. 
- DL은 id를 가지고 클래스를 찾아주는 과정이다.
- ![](https://lh3.googleusercontent.com/proxy/cV-ZFatg64RArNwXlCLFFZf1pQb4PZFN8PCF6OwUWQV7Mg3S2SeLyfKM9G19fWXjN4h9gDWMXvZly64Eq7jQb82QQsnzMjg6nfQzd0mBNzyfPurPJzrbHlcCnKYMrweaiC2mK2ixXNxjQE9nuEPHP2i5oU3p7WuKBWL7YLkwTVHzM334LY73vJI3Y6b51YPqu9fd9JrEExl_R2hD_gKV8Q44A2yFmFKGTr0wRU_sFXD3h83PJPNXVMU1bfO4hm1VVQIwzTKTRQG_ymajHvBuVOd0kSSFFb9HNQ)


### DI방식 - (1) 생성자 방식 , (2) 세터 메서드 방식
- 등장배경 : 점점 많은 프로그램의 xml파일이 추가되어서 복잡도가 증가하기 때문이다.

- VO는 스프링에서 관리 대상 클래스가 아니다 : 사용자 정의 데이터형

- 스프링에서 관리하는 대상은 Bean이다.
  - @Repository : DAO
  - @Component : Manager(웹크롤링,트위터,openAPI)
  - @Controller : Model
  - @Service : Service (DAO + DAO = BI)
  - @Aspect : 공통모듈
  
- 스프링은 기능이 있는 데이터만 관리하고, DB는 관리하지 않는다.


### @Configuration , @Bean
- 스프링 컨테이너가 생성한 빈은 싱글톤 객체이다. 
- 그래서 스프링 컨테이너는 @Bean이 붙은 메서드에 대한 한 개의 객체만 생성한다. 
- 즉, 몇 번을 호출하더라도 항상 같은 객체를 리턴하다는 것이다. 

### 두 개 이상의 설정파일이 존재할 때 처리하는 방식 (=xml 여러 개로 분산된 경우)
- app1.xml (사원) , app2.xml (멤버) 이 존재하는 경우
- 작업하는 사람이 7명이면, 설정파일이 8개이다.

- xml은 배열 형식으로 설정파일을 받는다.
- 어노테이션은 배열로 받지 않아도 된다.



- return형이 object니까 반드시 형변환을 해야한다.
- getBean은 저장되어 있는 객체 주소를 받아오는 것임






### DAO
- spring에서는 sqlSession을 생성하지 않아도 된다.
- COMMIT을 따로 설정하지 않아도 된다. 

### app3.xml
- 데이터베이스 정보 생성하기
  - BasicDataClassName 메서드 : driverClassName , url , username , password
- SqlSessionFactory(ssf)
- EmpDAO로 전송하기

```tip
1. 일반 데이터 값 주입 : `p:변수명=""`
2. 스프링에서 생성된 객체 주소 주입 : `p:객체명-ref="해당 클래스의 id명"`
```


