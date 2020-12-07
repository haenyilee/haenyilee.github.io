# 20201116-tilesProject (1)

## SB Admin
- free download


## tiles
- 스프링 타일즈는 뷰단의 탑,사이드메뉴,하단,메인 등을 페이지 include 방식으로 나누는 기존구조를 쉽게 적용하기 위한 템플릿 프레임워크이다.

장점은 include 디자인을 변경하면 페이지를 전체적으로 수정해야하는 번거로움을 없애고



출처: https://epthffh.tistory.com/entry/스프링-타일즈-Spring-Tile-설정해보기 [물고기 개발자의 블로그]

### main.jsp

- `<%@ taglib prefix="tiles" uri="http://tiles.apache.org/tags-tiles" %>`
- `<tiles:insertAttribute name="header"/>`
- apache tiles 설정

### tiles.xml

- dtd 설정 : `<!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 3.0//EN" "http://tiles.apache.org/dtds/tiles-config_3_0.dtd">`
- jsp 파일 등록 : `<definition name="main" template="/WEB/INF/main/main.jsp">`
  - name이 return값이다. 
- 태그 등록 : `<put-attribute name="header" value="/WEB/INF/main/header.jsp"></put-attribute>`
- tiles의 역할은 include와 같다. 

- 공통 형식 만들기
  - 경로 2개 길이일때
  
  ```
  <definition name="*/*" extends="">
      <put-attribute name="content" value="/WEB-INF/{1}/{2}.jsp"></put-attribute>
  </definition>
  ```

  - 경로 3개 길이일 때
  
  ```
  <definition name="*/*/*" extends="">
      <put-attribute name="content" value="/WEB-INF/{1}/{2}/{3}.jsp"></put-attribute>
  </definition>
  ```  

### application-context.xml
- context
- viewResolver
  - `p:order="1"` : viewResolver를 여러개 쓸 때의 순서이다. 
  - 팝업창을 띄울때는 tiles가 적용이 안되기 때문에 viewResolver가 필요하다.
  - 우선순위를 결정할 때 사용하는 것이 order이다. (0부터 시작)
  - 화면변경되는 tiles가 0번으로 실행되도록 설정한다. 
  - 우편 번호 검색 창 같이 팝업창을 새로 띄우면 tiles 적용이 안된다. 이럴 때 viewResolver를 사용하는 것이다.
  - `return "main/main"` 이면 viewResolver가 작동된다. 
  
- tilesResolver
  - tiles와 apache를 연동하는 과정임
  - `UrlBasedViewResolver` : return값을 받음
  - `TilesView` : return값을 넘겨받아서 include를 시켜준다.
  - `return "main"` 이면 tilesResolver가 작동된다. 
