---
sort: 2
---

# MVC구조로 게시판 만들기 (4) 새글 작성

## [applicationContext.xml]

- `insert.do` , `insert_ok.do` 등록하기

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans>
	<bean id="list.do" class="com.sist.model.ListModel"/>
	<bean id="detail.do" class="com.sist.model.DetailModel"/>
	<bean id="insert.do" class="com.sist.model.InsertModel"/>
	<bean id="insert_ok.do" class="com.sist.model.InsertOkModel"/>
</beans>
```



## [mapper]
## [DAO]
## [Model]

- **InsertModel.jsp**
- 구현해야할 기능이 없음
- insert.jsp로 화면만 전환하면 됨




```java
return "board/insert.jsp";
```

- **InsertOkModel.jsp** 

```note
### 화면을 이동하는 방법
1. **sendredirect** : request가 초기화되는 상태이다. : `return "redirect:list.do";`
2. **forward** : request를 전송하기 때문에, jsp에서 request에 담은 데이터를 받아서 출력할 수 있다. : `return "board/list.jsp";` 

```


```java
package com.sist.model;

import javax.servlet.http.HttpServletRequest;

public class InsertOkMode implements Model {

	@Override
	public String handlerRequest(HttpServletRequest request) {
		// TODO Auto-generated method stub
		return "redirect:list.do";
	}

}
```



## [Controller]
## [View]

```note
1. `list.jsp` : 받을 데이터가 없는 상태에서 HTML만 출력할 때 사용한다. 
2. `list.do`  : 받을 데이터를 [Controller >> ListModel >> list.jsp]를 거쳐서 전송한 후 출력할 때 사용한다.
```


- **insert.jsp**

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<style type="text/css">
.row {
   margin: 0px auto; /*가운데 정렬*/
   width:700px;
}
h1 {
     text-align: center;
}
</style>
</head>
<body>
  <div class="row">
   <h1 class="text-center">글쓰기</h1>
   <form method="post" action="insert_ok.do">
   <table class="table table-hover">
     <tr>
       <th class="danger text-right" width=15%>이름</th>
       <td width=85%>
         <input type=text name=name size=15 class="input-sm">
       </td>
     </tr>
     <tr>
       <th class="danger text-right" width=15%>제목</th>
       <td width=85%>
         <input type=text name=subject size=45 class="input-sm">
       </td>
     </tr>
     <tr>
       <th class="danger text-right" width=15%>내용</th>
       <td width=85%>
         <textarea rows="10" cols="50" name=content></textarea>
       </td>
     </tr>
     <tr>
       <th class="danger text-right" width=15%>비밀번호</th>
       <td width=85%>
         <input type=password name=pwd size=10 class="input-sm">
       </td>
     </tr>
     <tr>
       <td colspan="2" class="text-center">
         <input type=submit value=글쓰기 class="btn btn-sm btn-primary">
         <input type=button value=취소 class="btn btn-sm btn-primary"
           onclick="javascript:history.back()"
         >
       </td>
     </tr>
   </table>
   </form>
  </div>
</body>
</html>
```

- **insert_ok.jsp**
