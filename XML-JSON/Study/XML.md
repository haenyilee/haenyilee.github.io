---
sort: 2
---

# XML(1)

### XML이란?
- HTML은 웹 페이지에서 데이터베이스처럼 구조화된 데이터를 지원할 수 없지만 XML은 사용자가 구조화된 데이터베이스를 뜻대로 조작할 수 있다. 
- XML은 데이터를 문서로 저장해서 오라클 없이도 데이터를 가져와서 쓰기 위함이 기본 목적이다.

### XML을 사용하는 이유는?
- 일반 TXT파일은 데이터 내용을 구분할 수 없다.
- 이때 내용을 구분할 수 있게 하는 형식이 탄생했는데 그것이 XML과 JSON이다.
- 그 중 XML은 포맷형식이 운영체제마다 동일하기 때문에, 어떤 운영체제를 쓰던지 동기화 가능하다.
- 기존의 모든 프레임워크들은 Spring도 있고 MyBaits, IBatis, Hibernate들이 있는데,
<br> 이들이 전부 XML을 사용한다.
- Spring과 가장 어울리는 것이 MyBatis이다.
- Struts와 가장 어울리는 것은 Hibernate이다.

### XML파싱이란?
- 태그의 속성값을 읽어 오는 과정

### XML 파싱의 종류
#### 1. JAXB
- 외부에서 데이터를 XML로 보내는 경우에 주로 사용함
- Java와 XML을 연결하는 라이브러리임

#### 2. JAXP
(1) DOM
- XML을 메모리에 저장하고 제어함
- 수정, 삭제, 추가, 검색이 가능함
- 단점 : 속도가 늦음
- XML을 오라클 대신 사용할 때 주로 사용함
- 거의 사용하지 않음

(2) SAX
- 검색만 가능함(데이터 읽기만 가능)
- MyBatis와 Spring이 주로 사용함
- 기억해야 하는 이유 : 에러가 주로 SAX에서 발생하기 때문이다.


### XML태그란?
- 사용자 정의 태그이다. 
- 고정태그가 아니기 때문에 어떤 목적으로 만들었는지 알 수 없다.
- 업체에서 제공하는 XML태그만 사용해야 한다.
- MyBatis는 구글에서 인수했기 때문에, 구글에서 제공하는 XML 태그나 속성만 사용해야 한다. 
- XML태그와 속성이 어떤게 있는지 알 수 없기 때문에, 
<BR>어떤 태그가 있는지 태그나 속성의 목록을 제공해주는 것이 DTD파일이다.

### XML태그의 구조
- 태그명+구조

(1)
```
<태그>데이터저장</태그> 
<td>데이터 출력</td> ==> text()
```
(2) 
```
<태그속성="데이터 저장"/>
<img src=""/> ==> attr("src")
```
- 문법사항은 DTD안에 있는 태그나 속성만 사용해야 한다.
- HTML은 자유롭게 사용할 수 있다는 점이 차이점이다.





### XML 데이터 읽는 방법
#### 1. JAXB
(1) 자바 클래스와 XML의 태그를 매칭시켜야 한다.
- 태그 안에 태그가 있는 경우에는 자바 클래스로 제작해야 한다.
- 태그 안에 문자열(데이터)가 있는 경우에는 자바 변수로 제작해야 한다.
- XML데이터를 자바변수에 저장하는 방법을 '**언마샬**'이라고 한다.
- 자바 변수를 XML데이터로 출력하는 방법을 '**마샬**'이라고 한다.
- 데이터가 오라클에 저장되어 있으면 실시간 변경을 할 수 없다. 
- 실시간 변경을 위해서는 웹사이트를 크롤링해야하는데 이때 알아야 하는 것이 XML이다.
- 데이터를 읽어와서 출력하는데 사용자가 보기 쉽게 하는 것을 CSS라고 한다.

```xml	
<rss>						<!--Rss클래스-->
	<channel>				<!--channel클래스-->
		<item>				<!--item클래스-->

			<title>aaa</title>	<!--title변수-->
			<img>aaa</img>		<!--img변수-->
			<link>aaa</link>	<!--link변수-->

		</item>
	</channel>
</rss>
```

(2) 루트 클래스 = Rss클래스 제작
- rss태그가 전체 태그를 감싸고 있기 때문에 root임
```java
package com.sist.xml;

import javax.xml.bind.annotation.XmlRootElement;

@XmlRootElement
public class Rss {
	private Channel channel= new Channel();

	public Channel getChannel() {
		return channel;
	}

	public void setChannel(Channel channel) {
		this.channel = channel;
	}
	
}
```
(3) Channel클래스 제작
- 밑에 50개의 Item 태그를 가지고 있기 때문에 List에 데이터를 모아주어야 한다.
```java
package com.sist.xml;

import java.util.*;

public class Channel {
	private List<Item> list = new ArrayList<Item>();

	public List<Item> getList() {
		return list;
	}

	public void setList(List<Item> list) {
		this.list = list;
	}
	
}
```

(4) Item클래스 제작
```java
package com.sist.xml;

public class Item {
	private String title;
	private String description;
	private String author;
	private String link;
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getDescription() {
		return description;
	}
	public void setDescription(String description) {
		this.description = description;
	}
	public String getAuthor() {
		return author;
	}
	public void setAuthor(String author) {
		this.author = author;
	}
	public String getLink() {
		return link;
	}
	public void setLink(String link) {
		this.link = link;
	}

}
```

(5) XML에 있는 데이터를 채우는 클래스 제작
```java
package com.sist.xml;
import java.util.*;

import javax.xml.bind.JAXBContext;
import javax.xml.bind.Unmarshaller;

import com.sun.jmx.remote.internal.Unmarshal;

import java.net.*;

public class NewsManager {
	public List<Item> newsAllData(String fd)
	{
		List<Item> list = new ArrayList<Item>();
		
		try {
			// 1. 웹사이트에 연결
			/*
				where=rss(XML로 전송 받는 부분)
				query=(사용자가 보내준 검색이를 받는 부분) 
					- 한글을 보내게 되면 반드시 인코딩해서 보내야함
					- 인코딩 할 때 사용하는 api : import java.net.*
					- 웹서버에 데이터 전송 시 인코딩해서 전송하는 메소드 : URLEncoder
			 */
			String strUrl="http://newssearch.naver.com/search.naver?where=rss&query="
					+ URLEncoder.encode(fd,"UTF-8");
			
			// 2. 실제 연결하는 클래스 : URL
			URL url= new URL(strUrl);
			
			// 3. 명령을 내리기
			/*
				- @XMLRootElemnet가 붙어 있는 클래스를 보내야함
			 */
			JAXBContext jb=JAXBContext.newInstance(Rss.class);
			
			// 4. 파싱을 해서 데이터를 읽어온 뒤, 자바 클래스에 값을 넣어주도록 설정 요청
			Unmarshaller un=jb.createUnmarshaller();
			
			// 5. 값을 채워주기
			Rss rss= (Rss) un.unmarshal(url);
			list=rss.getChannel().getItem();
			
			
			
			
			
		} catch (Exception e) {}
		
		return list;
	}
}
```
(6) news.jsp 제작
```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" import="java.util.*, com.sist.xml.*"%>

<%
	// 데이터 읽어가는 과정 및 순서
		// 6. 한글을 한글로 변환(디코딩)
		request.setCharacterEncoding("UTF-8");
	
		// 1. 사용자가 보내준 검색어 읽어오기
		String fd=request.getParameter("fd");
		// 2. 주의점 : 맨 처음에는 검색어를 보내줄 수 없어서, default로 설정해야함
		if(fd==null)
		{
			fd="맛집";
		}
		// 3. 네이버로 연결해서 데이터를 읽어오는 역할의 클래스 
		NewsManager m = new NewsManager(); 
		
		// 4. 네이버로부터 데이터 수집
		List<Item> list = m.newsAllData(fd); 
		
		// 5. 화면에 출력하기

%>    

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<style type="text/css">
/* 
	<오버라이딩>
	이미 만들어진 css를 변경하는 부분
*/
.row{
	width:800px;
	margin:0px auto; /* 가운데 정렬 */
}
h1{
	text-align: center; /* 문자열을 가운데 정렬 */
	

}
.title{
	font-size: 25px;
	color: #FC6;
}

</style>

</head>
<body>
	<!-- 1. 전체 테두리 만들기 : 960px (양쪽에 약간의 여백을 가지고 있음)-->
	<div class="container">
		<!-- 2. 타이틀 만들기 -->
		<div class="row">
			<h1>네이버 실시간 뉴스 검색</h1>
			<!-- 3. 검색창 만들기 -->
			<table class="table">
				<tr>
					<td>
					<form method="post" action="news.jsp">
					<input type=text name=fd size=15 class="input-sm">
					<input type=submit value="검색" class="btn btn-sm btn-primary">
					</form>
					</td>
				</tr>
			</table>
			<!-- 4. 뉴스 데이터를 출력하는 위치 -->
			<table class="table table-striped">
				<tr>
					<td>
						<%
							for(Item i:list)
							{
						%>
								<table class="table table-hover">
									<tr>
										<td class="text-center title">
										<%=i.getTitle() %>
										</td>
									</tr>
									<tr>
										<td><%=i.getDescription() %></td>
									</tr>
									<tr>
										<td>
										<a href="<%=i.getLink()%>">
										<%=i.getDescription()%>
										</a>
										</td>
									</tr>
									
									<tr>
										<td class="text-right">
										<%=i.getAuthor() %>
										</td>
									</tr>
								</table>
						<%
							}
						%>
					</td>
				</tr>
			</table>
		</div>
	</div>
</body>
</html>
```

#### 2. DOM
(1) 테이블과 동일한 형식으로 XML파일 제작
```XML
<?xml version="1.0" encoding="UTF-8"?>
<sawon>						<!-- 테이블명 -->
	<list>					<!-- Record, Row -->
		<sabun>1</sabun>	<!-- Column명 -->
		<name>홍길동</name>
		<dept>개발부</dept>
		<job>사원</job>
		<sal>3000</sal>
	</list>
	<list>
		<sabun>2</sabun>
		<name>심청이</name>
		<dept>개발부</dept>
		<job>대리</job>
		<sal>3800</sal>
	</list>	
	<list>
		<sabun>3</sabun>
		<name>박문수</name>
		<dept>총무부</dept>
		<job>과장</job>
		<sal>4500</sal>
	</list>	
	
</sawon>
```

#### 3. DOM : 속성값을 불러오는 방법
(1) XML 데이터 만들기
```xml
<?xml version="1.0" encoding="UTF-8"?>
<sawon_list>
	<sawon sabun="1" name="홍길동" dept="개발부"/>
	<sawon sabun="2" name="홍길은" dept="총무부"/>
	<sawon sabun="3" name="홍길금" dept="기획부"/>
	<sawon sabun="4" name="김길동" dept="영업부"/>
	<sawon sabun="5" name="이길동" dept="자재부"/>	
</sawon_list>
```

(2) 만든 XML 데이터 읽어오기
```java
package com.sist.dom;
import java.io.*;
import javax.xml.parsers.*;
import org.w3c.dom.*;

public class DomManager2 {
	
	public static void main(String[] args) {
		try {
			
			// 1. 파서기를 생성 
			// * 파서기의 역할 : xml데이터를 메모리에 트리형태로 저장하는 장치
			DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
			
			// 2. 파서기를 이용하기
			DocumentBuilder db = dbf.newDocumentBuilder();
			
			// 3. XML을 읽어서 트리형태로 메모리에 저장하기
			Document doc=db.parse(new File("C:\\webDev\\20200917-JxspStudy1\\src\\com\\sist\\dom\\sawon2.xml"));
			
			// 4. 최상위 태그(XMLRootElement)가져오기
			// * 최상위태그=테이블명 *
			Element root=doc.getDocumentElement();
			System.out.println(root.getTagName());
			
			// 5. sawon태그를 묶어서 처리하기
			NodeList list=root.getElementsByTagName("sawon");
			System.out.println(list.getLength());
			
			// 6. for문 돌려서 속성값들 가져오기
			for(int i=0;i<list.getLength();i++)
			{
				// (1) 사원태그를 한 개씩 읽어오기
				Element sawon=(Element)list.item(i);
				String sabun=sawon.getAttribute("sabun");
				String name=sawon.getAttribute("name");
				String dept=sawon.getAttribute("dept");
				
				System.out.println(sabun+" "+name+" "+dept );
				
			}
			
		} catch (Exception e) {}
	}
}

```
#### 4. SAX
(1) XMLParser 클래스 제작
- SAXParser 재정의? 오버라이딩?
```JAVA
package com.sist.dom;

import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

public class XMLParser extends DefaultHandler{
	
	@Override
	public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
		// System.out.println(qName+"태그 읽기 시작");
		if(qName.equals("sawon"))
		{
			String sabun=attributes.getValue("sabun");
			String name=attributes.getValue("name");
			String dept=attributes.getValue("dept");
			System.out.println(sabun+" "+name+" " +dept);			
		}
		
		// XML읽을 때, 태그명과 속성명이 일치해야 함!
	}

	@Override
	public void startDocument() throws SAXException {
		System.out.println("XML문서가 시작!!");
	}

	@Override
	public void endDocument() throws SAXException {
		System.out.println("XML 문서 읽기 종료!!");
	}


	@Override
	public void endElement(String uri, String localName, String qName) throws SAXException {
		System.out.println(qName+"태그 읽기 종료");
	}

	
}

```

(2) SAXManager 클래스 파일 제작하기
```java
package com.sist.dom;

import java.io.*;

import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;

public class SAXManager {

	public static void main(String[] args) {
		try {
			// 1. SAX파서기를 생성하는 클래스 만들기
			SAXParserFactory spf=SAXParserFactory.newInstance();
			
			// 2. SAX파서기 가져오기
			SAXParser sp=spf.newSAXParser();
			
			// 3. 파일 읽기 요청하기
			XMLParser xp=new XMLParser();
			
			// 
			sp.parse(new File("C:\\webDev\\20200917-JxspStudy1\\src\\com\\sist\\dom\\sawon2.xml"), xp);
			
		} catch (Exception e) {}
	}

}

```


### 메모리에 저장되는 형식
![](https://lh3.googleusercontent.com/proxy/IrcF0WgeDs3SPGPqWcwwMuAuYHoImnsz3vKA59LXDzR9uO7iCQvLaCKG5zJhVliaeWXTyEF9vdj_YmReeaPE0AgLXB3rYZTWC-bfOwgtY8thhXCjjWzmCnnoEmdiijY)
#### 1. List 방식
- 일반 변수, 클래스
#### 2. Tree 방식
- DOM파싱하면 트리 형식으로 저장됨
- Document
- 태그는 Elemnet
- NodeList
- TextNode
- ML (HTML , XML)
- Document Object Model (DOM)
- Simple API for XML (SAX)
