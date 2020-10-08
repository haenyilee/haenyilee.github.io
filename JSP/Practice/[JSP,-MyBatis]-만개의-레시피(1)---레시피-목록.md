---
sort: 2
---

# RecipeModel.java

## import
```
java
import com.sist.dao.*;
import java.util.*;
```

## request
- 사용자가 요청한 값 받기

# recipe.jsp
## model.java import

## recipeModel 메모리 할당하기
- useBean태그를 사용함

## model.java에서 넘겨준 request받기
-  값을 받을 때, 일반변수는 받을 수 없고 request,session 등으로 넘겨진 값만 받을 수 있다.

## css/html
- 여백 화면
```html
<div class="container">
```
- 풀화면
```html
<div class="container-fluid">
```

## c:forEach 태그로 데이터 출력하기

## 이전 다음 페이지 버튼 만들기
- 1보다 크면 이전 버튼 누르면 -1 페이지
- 마지막 페이지보다 작으면 다음버튼 눌렀을때 +1 페이지
