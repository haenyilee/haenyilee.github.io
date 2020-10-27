---
sort: 9
---

# AutomaticDI (3)
- 책 : 132 page


## 스프링 어노테이션

### 주입
- `@Autowired`
  - 스프링에서 저장된 클래스 중에서 맞는 주소 찾아서 자동 주입
  - 메모리에 저장된 객체 주소를 가져올 때 사용함
  - 스프링에서 자동으로 찾아옴
  - 일반 변수는 사용하지 않음
- `@Resource`
  - 특정 객체를 지정할 경우 사용함
  
- `@Qualifier` 
- `@Inject`

### 메모리 할당
- `@Component` : 일반 클래스 (VO , MainClass)
- `@Repository` : DAO
- `@Controller` : Model
- `@Service` : Manager , DAO+DAO+...
- `@RestController` : 파일명 전송 없이 일반 문자열을 전송할 때 사용함 (Ajax, React)

```tip
**면접 단골질문**
1. DI와 Contatianer의 개념
2. AOP vs OOP
3. DAO vs Service
```

## 빈 라이프사이클
### 1단계 : [객체생성] 클래스 > 메모리할당 > 컨테이너
- 스프링에서 메모리할당을 해서 생긴 저장공간이 바로 Container이다.
- 대표적인 Container는 ApplicationContext이다.
  - Map 형식의 저장공간이다.
  - `@Component`가 
  
  |key|Valule|
  |---|------|
  |sawon|new Sawon()|
  |mem|new Member() = app.getBean("mem")|

### 2단계 :[의존설정] DL > DI
- DL : 클래스 찾기
- DI : 생성과 동시에 필요한 데이터가 있느 경우
  - 처리 : DI setter(p:) / constructor (c:)

```Danger
**app.getBean("mem")** 이란 무엇인가?
**getBean("mem")**
- DL이다
- Dependency Lookup함수이다.?
```
### 3단계 : [초기화, 소멸] 활용 > 메모리해제
- 활용 : 프로그래머가 필요한 기능을 코딩하는 과정
- 소멸 : 활용이 끝났으니 메모리 해제하라고 명령을 내리는 과정

  
- AnnotaionConfigApplicationContext
- MVC구조 웹에서는 WebApplicationContext이다.

## 실습해보기 (1)

### [VO]

- 변수 초기화
- 은닉화
- `@Componenet`

### AppConfig.java
- `@Configuration`
- `@ComponentScan(basePackages={"com.sist.di"})`
  - 배열 형태로 되어 있기 때문에 `{}`안에 여러개의 패키지를 등록할 수 있음

- app.xml을 대체하는 클래스
- mainClass에서 어노테이션을 활용함


### MainClass
- `@Component("mc")`
  - 메모리할당을 하되 id를 mc로 지정한다.
- `@Autowired`

- `AnnotationConfigApplicationContext app= new AnnotationConfigApplicationContext(AppConfig.class);`
  - 5.0 버전에서 사용하는 방식
  
## 실습해보기 (2)
- resouce로 주입받기

### [VO]
- 패키지를 달리해서 제작함
  - [com.haeni.di2] - Sawon.class
  - [com.haeni.di3] - Member.class
- 메모리할당
  - `@Component`는 필수사항이 아님
  
### AppConfig
- [com.haeni.di4]
- `@Configuration`
- `@ComponentScan(basePackages={"com.haeni.di2","com.haeni.di3","com.haeni.di4"})`
  - 전체 패키지를 전부 등록하려면 `@ComponentScan(basePackages={"*"})`로 작성하면 됨

### MainClass
- [com.haeni.di4]

- [의존 설정]
  - `@Resource`로 주입할때는 id명칭을 꼭 메모리할당할 때 지정했던 id(`@Component="id"`)와 일치해야함
  - 컨테이너 초기화 전까지는 의미 없음 
  - 컨테이너가 패키지 읽고 어노테이션 확인해서 값을 넣어주게 됨
  
```java
@Component
public class MainClass {
  @Resource(name="sa")
	private Sawon sa;
	@Resource(name="mem")
	private Member mem;
}
```


- [초기화]

```java
public static void main(String[] args) {
		AnnotationConfigApplicationContext app= new AnnotationConfigApplicationContext(AppConfig.class);
		MainClass mc= (MainClass)app.getBean("mc");
}
```

- [소멸] : `app.close();`


## 실습해보기 (3) - 빈 객체의 초기화와 소멸
- [com.haeni.di5]
- `InitializingBean`, `DisposableBean`활용하기
	- `InitializingBean` : 초기화 
	- `DisposableBean` : 소멸
- 컨테이너가 가지고 있다가 `destroy()`함수 호출되면 소멸


### Sawon.java

```java
package com.haeni.di5;

import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class Sawon implements InitializingBean,DisposableBean {
	private String name;
	private String dept;
	
	public Sawon(String name, String dept)
	{
		this.name=name;
		this.dept=dept;
	}
	public void print()
	{
		System.out.println("이름:"+name);
		System.out.println("부소:"+dept);
	}
	@Override
	public void destroy() throws Exception {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void afterPropertiesSet() throws Exception {
		// TODO Auto-generated method stub
		
	}

}
```

- 초기화되는 순서
  - 생성자 호출
  - setterDI : 멤버변수에 값을 채운다 , 메모리 할당
  - afterPropertiesSet() => setName, setDept
  - print() => 프로그래머가 활용
  - close() => destry()


### app.xml

- `scope="prototype"` : 사용범위
	- singleton(메모리주소 같음)
	- prototype(메모리 주소 다름)
