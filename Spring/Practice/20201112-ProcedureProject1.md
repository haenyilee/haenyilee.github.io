---
sort: 2
---

# 스프링에 PROCUDURE 적용해보기

- 마이바티스가 아닌 일반 dao를 만들 것임
  - 프로시저 호출 방법을 알기 위해서
  
## 초기 셋팅

### application-context.xml

- `context` : 패키지 메모리 할당
- `viewResolver` : JSP 찾기


### StudentVO.java
- DTO : 사용자 정의 데이터형 
- total , avg : 사용자 정의 FUNCTION으로 만들어야 한다.
  - INSERT, UPDATE, DELETE 기능은 PROCEDURE로 만들어야 한다.


### MainController.java

- `@Controller` : RequestMapping을 찾아줘야 하니까 메모리 할당을 해야 한다. 
- MVC구조의 Model역할이다. 
  - 요청을 처리하고 결과값을 전송해주는 역할을 담당한다.
  
### StudentDAO.java
- `@Repository` : DB와 관련된 DAO의 메모리 할당하기 위해 사용한다.

- `CallableStatement` : Procedure를 호출할 때 사용한다.
  - `PreparedStatement` : 일반 sql문장을 전송할 때 사용한다.
  - 마이바티스를 사용할 때에도 디폴트로 잡혀있는 PreparedStatement를 CallableStatement로 변경해야 한다.
- 드라이버 등록 , 연결 , 해제



- pom.xml에서 ojdbc 에러가 발생하는 이유는 maven repository에서 삭제되었기 때문이다.
	- 그래서 직접 ojdbc14를 다운로드 받아서 lib폴더에 넣어줘야 한다.

```tip
**메이븐 repository에 없는 외부 라이브러리 추가하기**
- WEB-INF - [lib]에 jar파일 추가하기
- pom.xml에 추가된 jar파일 등록하기
```

- studentListData 클래스 제작 

- `registerOutParameter` : 저장공간 만들기
- `OracleTypes.CURSOR` : select의 결과를 cursor가 갖는다

```note
**OracleTypes의 유형**
- int : OracleTypes.INTEGER
- string : OracleTypes.VARCHAR
- double : OracleTypes.DOUBLE
- Date : OracleTypes.DATE
- ResultSet : OracleTypes.CURSOR
```



### MainController.java

- DAO 객체 받기
- RequestMapping으로 JSP에 결과값 전송하기

- 프로시저 작동 과정은 볼 수가 없음
	- 호출된 결과로 유추할 수 있어야 한다.
	- 만들어진 프로시저를 활용할 수 있어야 한다.
	
### list.jsp

- studentListData 결과 화면에 출력하기

### StudentDAO.java : studentInsert

- `cs.executeQuery();` : 프로시저의 실행은 무조건 executeQuery이다.
	- commit은 프로시저에 작성해두었기 때문에 따로 또 실행하지 않아도 된다. 
	
	
### MainController.java : main_insert , main_insert_ok

- insert 실행 후, 완료되면 `redirect:list.do` 로 응답한다.

### list.jsp
- 총점, 평균 컬럼 추가
- 수정/삭제 버튼 컬럼 추가
- 등록 버튼 추가


### insert.jsp

- 학생이름, 과목별 성적 등록하는 화면 출력하기
- html 5버전
