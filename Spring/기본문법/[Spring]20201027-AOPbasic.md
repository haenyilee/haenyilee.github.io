# 20201027-AOPbasic (1)

## AOP개념
- 반복되는 공통모듈을 자동으로 호출하게 하는 콜백함수?
- Aspect Oriented Programming
- Aspect : 공통으로 사용되는 것을 모아둔 것
- Transaction(일괄처리) , 보안

## MainClass.java

## app.xml

## MyDAO.java

## MyAspect.java
- `@Aspect` : 공통모듈임
- `@Component` : 메모리할당은 못하기 때문에 따로 처리해줘야 함

```NOTE
**AOP 호출위치 = JoinPoint**
- Before : 핵심기능 시행 전 
- Around : 핵심기능 = JoinPoint
- After-Throwing : 예외처리
- After : 핵심기능 시행 후
- After-Returning
```

- AOP 적용되는 메소드 설정 : PointCut
- Advice 여러개 = Aspect


- 리턴형 관계없이 처리하기 : 
-
`db_*(int)`
`db_*(..)`
`db_*()`

## Proxy 패턴
- 대신 호출하는 대체제 과정
- 대체자
- autoproxy
- 콜백함수처럼 보일 수 있게 해줌
- 게시판 수정,삭제할때 트랜젝션에 사용해야 하기 때문에,,
