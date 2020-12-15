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
- 자바 컬렉션 종류
![](https://t1.daumcdn.net/cfile/tistory/99B88F3E5AC70FB419)

```

- 파싱 : mashaller,unmarshaller

```tip
**[면접]마샬vs언마샬**
- Unmarshaller : 
- Mashaller : 
```


- @Component("mgr") : id를 설정해줌
  - 설정 안해주면 앞글자 소문자로 변환해서 자동 
  
  
### MainController
- 뉴스 검색 결과를 view로 전송해줌

- news.do => fd ==null 인경우
- news.do?fd= => fd.equals("")인 경우


### news.jsp
- 화면 출력
- 검색어를 form태그로 전송함

- 클릭했을 때 별도의 창을 띄우려면 : a태그에 `target="_blank"`속성 주기
  - 코틀린에서는 webview
  
  
### MainController
- 코틀린으로 데이터 전송

## kotlin

### 메뉴달기

#### MainActivity
- onCreateOptionsMenu
- onOptionsItemSelected
  
