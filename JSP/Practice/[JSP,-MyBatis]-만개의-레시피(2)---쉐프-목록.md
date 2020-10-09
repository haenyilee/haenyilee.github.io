---
sort: 2
---

# 만개의 레시피 (2) 쉐프목록 출력하기

## RecipeModel.java

- import
```
java
import com.sist.dao.*;
import java.util.*;
```

- request로 사용자가 요청한 값 받기

## recipe.jsp
- model.java import
- recipeModel 메모리 할당하기
  - useBean태그를 사용함

- model.java에서 넘겨준 request받기
  -  값을 받을 때, 일반변수는 받을 수 없고 request,session 등으로 넘겨진 값만 받을 수 있다.

- css/html
  - 여백 화면
```html
<div class="container">
```
  - 풀화면
```html
<div class="container-fluid">
```

- c:forEach 태그로 데이터 출력하기

- 이전 다음 페이지 버튼 만들기
  - 1보다 크면 이전 버튼 누르면 -1 페이지
  - 마지막 페이지보다 작으면 다음버튼 눌렀을때 +1 페이지

# 쉐프 이름 클릭하면 레시피 출력하기
## server.xml
- ```URIEncoding="UTF-8"```추가하기
```xml
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8"/>
```

## mapper.xml
- ```>```는 태그로 인식하기 때문에 ```&lt;```로 작성해야한다.
```
SELECT no,title,poster,chef,rownumm
FROM chef
WHERE chef=#{chef}
```

## DAO.java

## Model.java

## chef.jsp
- 쉐프 이름 클릭하면 chef_detail.jsp로 이동시키기
- <a>태그 사용하기

## chef_detail.jsp
- full화면으로 출력하기(여백x)
- 제목이 너무 기니까 fn활용해서 글자 20글자로 자르기
```jsp
<p>${fn:length(vo.title)>20?fn:substring(vo.title,0,20)+="...":vo.title }</p>
```

- model로 부터 request값 받기
- 페이지 이동 태그 만들기

