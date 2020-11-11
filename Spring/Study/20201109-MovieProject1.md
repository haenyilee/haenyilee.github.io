---
sort:
---

# 영화 입장권 통합전산망 메인페이지 만들기


## 초기 셋팅

### Properties 
- Java Compiler, Project Facets에서 JDK 1.8로 수정

### pom.xml 수정
- pom.xml이란 POM(Project Object Model)을 설정하는 부분으로 프로젝트 내 빌드 옵션을 설정하는 부분이다.
- Maven의 중요 설정파일들이 들어있다.
- 실제 라이브러리 파일이 저장되어 있는 저장소 서버의 위치를 지정하고, 사용할 라이브러리를 지정해준다.
- jar파일의 관리를 용이하게 만들어준다.

### web.xml
- init-param : DispatcherServlet 역할을 할 xml 파일의 경로를 지정해준다.
- DispatcherServlet이란? 
  - 클라이언트가 요청을 보내면 그에 따라 요청을 처리할 수 있는 곳으로 넘겨주고 그 결과인 서버쪽 응답을 클라이언트에게 넘겨줄 곳을 정한다. 

- Servlet Mapping이란? 
  - Servlet이 여러 개일 경우에, 브라우저가 정확히 어떤 Servlet을 요청하는지 찍어줄 필요가 있고, 고유한 이름이 있어야 Servlet을 구분이 가능하다. 
  - 원래의 Servlet의 구분을 위한 Full path는 매우 복잡한 URL의 형식이고, 보안에 취약하기 떄문에 Context path는 그대로 두고, 패키지명, Servlet이름을 간략하게 닉네임을 준다고 생각하고, 간결한 URL로 나타낸 것이 Mapping path이다.
  - Full Path는 일반적으로 `프로젝트명/ 파일명/ 패키지명/ Servlet파일명` 의 형식으로 구성된다.
  
  ```warning
  - Servlet Mapping할 수 있는 방법이 여러가지인가?
    - 첫 번쨰 방법이 web.xml이고
    - 두 번째 방법이 어노테이션?
  ```

## 데이터 파싱

### movie.json
- http://www.kobis.or.kr/kobis/business/main/searchMainDailyBoxOffice.do 에서 복사해온 데이터 임시 저장용 파일이다.

### MovieVO.java
- 변수 선언
- 캡슐화 , 은닉화

```tip
**캡슐화와 은닉화의 차이는?**
- 캡슐화 : 특정 목적을 위해 데이터와 데이터를 다루는 메서드를 묶어서 추상화하는 것 
- 은닉화 : 클래스의 속성들을 private로 만들어 클래스 밖에서 건드리지 못하게 하는 것
```

### MovieManager.java

- 웹사이트 내 JSON 데이터 파싱해서 List로 모으기

```note
**JSON**
- JSONArray
  - 배열 구조이다.
  - 배열 안에는 문자열, 숫자, 배열, 객체 등을 담을 수 있다.
  - [ ]의 대괄호를 이용하여 값들을 담으며, comma(,)로 그 값을 구분한다.
  - 후에 index를 이용하여 값을 꺼낼 수 있으므로 그 순서에 대한 고려를 해주어야 한다.
- JSONObject
  - 하나 이상의 key-value 쌍을 { }의 중괄호를 이용하여 담고있는 객체 구조이다.
  - key와 value 사이의 구분은 colon(:)으로 한다.
  - key-value 묶음간의 구분은 comma(,)로 한다.
  - 순서가 구분되지 않은 집합체이다.
```

- 일반 클래스일 경우에 `@Component` 로 메모리 할당한다.

```java
package com.haeni.manager;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.springframework.stereotype.Component;
import java.util.*;

@Component
public class MovieManager {
	
	// JSON 파싱
   public static void main(String[] args) {
      MovieManager m=new MovieManager();
      String json="";
      //String json=m.jsonAllDate(1);
      json=json.substring(json.indexOf("["),json.lastIndexOf("]")+1);
      try{
         JSONParser jp=new JSONParser();
         JSONArray arr = (JSONArray) jp.parse(json);
         //System.out.println("JSONArray => arr");
         //System.out.println(arr.toJSONString());
         for(int i=0;i<arr.size();i++)
         {
            JSONObject obj = (JSONObject)arr.get(i);
//            System.out.println((i+1)+":"+obj);
            System.out.println("영화명:"+obj.get("movieNm"));
            System.out.println("감독:"+obj.get("director"));
            System.out.println("장르:"+obj.get("genre"));
            System.out.println("등급:"+obj.get("watchGradeNm"));
            System.out.println("개봉일:"+obj.get("openDt"));
            System.out.println("줄거리:"+obj.get("synop"));
            System.out.println("============================================");
            
         }
      }catch (Exception e) {}
   }
   
   // JSP에 출력할 값 List에 채우기
   public List<MovieVO> jsonAllDate(int type)
   {
	  List<MovieVO> list=new ArrayList<MovieVO>();
      String url="http://www.kobis.or.kr/kobis/business/main/";
      switch(type)
      {
      case 1:
         url+="searchMainDailyBoxOffice.do";
         break;
      case 2: 
         url+="searchMainRealTicket.do";
         break;
      case 3: 
         url+="searchMainDailySeatTicket.do";
         break;
      case 4:
         url+="searchMainOnlineDailyBoxOffice.do";
         break;
      }
      try{
    	   Document doc=Jsoup.connect(url).get();
		   String json=doc.toString();
		   json=json.substring(json.indexOf("["),json.lastIndexOf("]")+1);
		   
		    JSONParser jp=new JSONParser();
			JSONArray arr=(JSONArray)jp.parse(json);
			System.out.println("JSONArray => arr");
			System.out.println(arr.toJSONString());
			for(int i=0;i<arr.size();i++)
			{
				JSONObject obj=(JSONObject)arr.get(i);
				//System.out.println((i+1)+":"+obj);
				MovieVO vo=new MovieVO();
				vo.setMno(i+1);
				vo.setRank((int)obj.get("rank"));
				vo.setTitle_ko((String)obj.get("movieNm"));
				vo.setTitle_en((String)obj.get("movieNmEn"));
				vo.setDirector((String)obj.get("director"));
				vo.setGenre((String)obj.get("genre"));
				vo.setPoster("http://www.kobis.or.kr"+(String)obj.get("thumbUrl"));
				vo.setGrade((String)obj.get("watchGradeNm"));
				vo.setRegdate((String)obj.get("openDt"));
				vo.setStory((String)obj.get("synop"));
				vo.setTime((String)obj.get("showTm"));
				vo.setRank_id((int)obj.get("rankInten"));
				vo.setNation((String)obj.get("repNationCd"));
				list.add(vo);
				/*System.out.println("영화명:"+obj.get("movieNm"));
				System.out.println("감독:"+obj.get("director"));
				System.out.println("장르:"+obj.get("genre"));
				System.out.println("등급:"+obj.get("watchGradeNm"));
				System.out.println("개봉일:"+obj.get("openDt"));
				System.out.println("줄거리:"+obj.get("synop"));
				System.out.println("====================================");*/
			}
      }catch (Exception ex) {}
      return list; 
   }
}
```
  
### MovieController.java
- Model이다.
  - JSP에서 HTML과 Java가 합쳐져 있던 것을 분리하여 Java부분만 별도로 코딩한 부분이 바로 Model이다.
  - HTML은 View에 해당된다.
- 기존의 Controller 역할을 담당하던 클래스는 스프링 안에서 자동으로 구현되어 있다.
- 메뉴얼을 만들 때 사용하는 것이 XML이다.
- `@Controller` : 이 클래스는 모델 역할이라고 스프링에 알려주는 것이 바로 `@Controller` 어노테이션이다.

- 매개변수 no는 int보다 String으로 받는 것이 좋다.
  - request로 값이 넘어오기 때문이다.

```java
package com.haeni.web;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.haeni.manager.MovieManager;
import com.haeni.manager.MovieVO;

@Controller
public class MovieController {
	@Autowired
	private MovieManager mgr;
	@RequestMapping("movie/main.do")
	public String movie_main(String no,Model model)
	{
		if(no==null)
			no="1";
		List<MovieVO> list = mgr.jsonAllDate(Integer.parseInt(no));
		model.addAttribute("list",list);
		return "movie/main";
	}
}
```

### application-context.xml
- ViewResolver(경로명, 확장자)
  - 데이터를 JSP로 전송


### main.jsp

### index.jsp
