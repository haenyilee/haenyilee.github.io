---
sort: 3
---

# HTML TAG

```

<!DOCTYPE html>
<html>
<head>
<meta charset=EUC-KR">
<title>Insert title here</title>
<!-- CSS / JavaScript -->

</head>
<body>
<!-- 
	화면출력 부분 ==> HTML : <> 태그+속성
	<문자 관련>
	<목록>
	<입력>
	<이동>
 -->

</body>
</html>

```

# 문자관련 태그
- ```<h1> ~ <h6>``` : 제목
- ```<strong>```, ```<em>``` : 강조체
- ```<front>``` : 글꼴 , 색상변경
- ```<p>``` : 단락
- ```<br>``` : 다음줄 출력
- ```<hr>``` : 수평선
- ```<pre>``` : 있는 그대로 출력
- ```<sup>``` : 위첨자

# 목록관련 태그
- ```<ul>``` : 순서가 없는 목록
- ```<ol>``` : 순서가 있는 목록
- ```<dl>``` : 데이터 목록 ```<dt>``` : 데이터 제목 ```<dd>``` : 데이터 설명
- ```<table>``` ```<tr>``` : 한줄을 만드는 경우 ```<th>``` : 테이블 제목 ```<td>``` : 데이터 첨부
  - ```<caption>``` : 테이블 제목

# 입력관련 태그
- ```<input type="">```
  - ```<input type="text">``` : 한줄 문자열 (id , 이름)
  - ```<input type="file">``` : 파일 업로드
  - ```<input type="password">``` : 비밀번호 입력
  - ```<input type="hidden">``` : 사용자는 볼 수 없고 데이터만 존재할 때
  - ```<input type="image">``` : button(submit) => 데이터 전송 버튼
  - ```<input type="date">``` : 달력
  - ```<input type="submit">```
  - ```<input type="button">```
```
<!-- name이 같으면 같은 그룹에 묶여서 중복체크 불가해짐 -->
성별:<input type=radio name=sex>남자<input type=radio name=sex>여자<br>
취미:<input type="checkbox">등산<input type="checkbox">개발
```


- ```<select>``` : 콤보박스
- ```<textarea>``` : 여러줄 문자열 입력

# 이동관련 태그
- ```<a href="이동할 파일명">``` : 일반 하이퍼링크
- ```<form action="이동할 파일명">```  : 데이터 전송과 동시에 이동

# 기타 태그
- ```<img>``` : 이미지출력
- ```<img src="파일 경로명">```
- ```<div>``` : 화면 분할 , 카드.... (table보다 시멘틱 언어임) => CSS
- ```<span>``` : 특정 부분만 변경 => CSS
