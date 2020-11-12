---
sort: 2
---

# 스프링에 PROCUDURE 적용해보기

- 마이바티스가 아닌 일반 dao를 만들 것임
  - 프로시저 호출 방법을 알기 위해서
  
## 초기 셋팅

### application-context.xml

- 패키지 메모리 할당 : `<context:component-scan base-package="com.sist.*"/>`
- JSP 찾기

```XML
<bean id="viewResolver" 
	class="org.springframework.web.servlet.view.InternalResourceViewResolver"
	p:prefix="/main/"
	p:suffix=".jsp"
/>
```



### StudentVO.java
- DTO : 사용자 정의 데이터형 
- total , avg : 사용자 정의 FUNCTION으로 만들어야 한다.
  - INSERT, UPDATE, DELETE는 PROCEDURE로 만들어야 한다.


### MainController.java

- `@Controller` : RequestMapping을 찾아줘야 하니까 메모리 할당을 해야 한다. 
- MVC구조의 Model역할이다. 
  - 요청을 처리하고 결과값을 전송해주는 역할을 담당한다.
  
### StudentDAO.java
- `@Repository` : DB와 관련된 DAO의 메모리 할당하기 위해 사용한다.

- `CallableStatement` : Procedure를 호출할 때 사용한다.
  - `PreparedStatement` : 일반 sql문장을 전송할 때 사용한다.
  - 마이바티스를 사용할 때에도 디폴트로 잡혀있는 PreparedStatement를 CallableStatement로 변경해야 한다.
