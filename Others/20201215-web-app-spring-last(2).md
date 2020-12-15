# 20201215-web-app-spring-last(2)

## com.sist.news

```note
**[면접]XML파싱법**
1. jaxp : Java API For XML Parser
  - DOM 
    - 단점 : 메모리에 파싱한 내용을 저장한 다음에 처리하기 때문에 속도가 느리다. 
    - 장점 : 수정, 추가 , 삭제 , 찾기가 가능하다 (문서형 데이터베이스)
  - SAX (일반 프레임워크) : Spring , MyBatis
    - 단점 : 읽기 전용이다.
    - 장점 : 태그를 한 개씩 읽어서 처리하는 과정이기 때문에 속도가 빠르다
2. jaxb : 빅데이터 , 공공포털 (OpenAPI:XML ,JSON)
  - 클래스와 XML태그를 매칭해서 처리한다.
```

```tip
**[면접]XML의 장점**
- 호환성이 좋다
- 모든 운영체제에 같은 내용으로 파싱된다.
```

### NewsManager
- 어노테이션 : @Component

```
@Component // 일반 클래스
public class NewsManager {}
```

- `import java.net.*;` : 웹을 연결할 때 URL인코더를 사용해야 해서 필요함



```note
**[면접]Collection(자료구조)**
- 자바에서는 표준화에 따라서 코딩을 해야 하는데, 데이터 저장방법에 따라 클래스가 분리된다.
- 
||종류|
|---|-------|||
|List|ArrayList|Vector|LinkedList|
|Set|ArrayList|Vector|LinkedList|
|Map|ArrayList|Vector|LinkedList|

```
