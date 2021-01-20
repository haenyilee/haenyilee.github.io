# R 활용해서 워드클라우드 만들기


## 네이버에서 워드클라우드 만들 텍스트 데이터 받아서 txt로 저장하기

```java
package com.sist.service;

import java.io.*;

import javax.xml.bind.JAXBContext;
import javax.xml.bind.Unmarshaller;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.springframework.stereotype.Component;

import java.util.*;

@Component
public class NaverManager {
	public void naverData(String fd) {
		ApiExamSearchBlog api = new ApiExamSearchBlog();
		
		String json = api.naverFindData(fd);
		
		
		System.out.println(json);
		
		// JSON 파싱 방법 ------------------------------------------------
		try {
			JSONParser jp = new JSONParser();
			JSONObject root = (JSONObject)jp.parse(json);
			JSONArray arr = (JSONArray)root.get("items");
			String result = "";
			for(int i = 0; i < arr.size(); i++){
				JSONObject obj = (JSONObject)arr.get(i);
				obj.get("description");
				result += obj.get("description") + "\r\n";
			}
			// 한글빼고 싹다 지우기 ----------------------------------------
			result = result.replace("<", "");
			result = result.replace(">", "");
			result = result.replace("/", "");
			result = result.replace("#", "");
			result = result.replace(".", "");
			result = result.replaceAll("[0-9]", "");
			result = result.replaceAll("[A-Za-z]", "");
			
			FileWriter fw = new FileWriter("c:\\upload\\naver.txt");
			fw.write(result);
			fw.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

## R실행시켜서 워드클라우드 만들기

- 참고 : https://m.blog.naver.com/PostView.nhn?blogId=bb_&logNo=220373069457&proxyReferer=https:%2F%2Fwww.google.com%2F


```java
package com.sist.web;

import org.rosuda.REngine.Rserve.RConnection;
import org.springframework.stereotype.Component;

@Component
public class RManager {
	
	public void graph(int no){
		
		// 워드클라우드 실행 순서 입력
		
		try {
			RConnection rc = new RConnection();
			
			// voidEval : 리턴 값이 없는 R 코드 실행
			// 라이브러리 불러오기
			rc.voidEval("library(rJava)");
			rc.voidEval("library(KoNLP)");
			rc.voidEval("library(wordcloud)");


			// 프로젝트의 리얼패스에 png파일 생성
			rc.voidEval("png(\"C:/springDev/springStudy/.metadata/.plugins/org.eclipse.wst.server.core/tmp0/wtpwebapps/Project/naver"+no+".png\")");
			
			// txt 파일을 줄마다 읽기
			rc.voidEval("data<-readLines(\"c:/upload/naver.txt\", encoding = \"UTF-8\")");

			// KoNLP의 extractNoun 이용해서 한글명사만 뽑아오기
			// USE.NAMES = F가 없는 경우 원문장, 명사뽑은 내용이 같이 나옴
			// 원문장 필료 없으므로 F
			// sapply 이용했으므로 vector로 반환
			rc.voidEval("data1<-sapply(data, extractNoun, USE.NAMES = F)");
			rc.voidEval("data2<-unlist(data1)");

			// 의미없는 한글자 제외시키기
			rc.voidEval("data3<-Filter(function(x){nchar(x)>=2}, data2)");

			// 단어들 몇 번 나왔는지 횟수 확인
			rc.voidEval("data4<-table(data3)");
			rc.voidEval("data5<-head(sort(data4, decreasing=T), 100)"); 

			// 워드 클라우드 그리기
			rc.voidEval("wordcloud(names(data5), freq = data5, min.freq = 3, max.words = 100, random.order = F, scale = c(10,1), colors = rainbow(15))");
			rc.voidEval("dev.off()");
			rc.close();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```


## Contoller

```java
package com.sist.web;

@Controller
public class Contoller {
	
	
	@Autowired
	private DAO dao;
	
	@Autowired
	private NaverManager nm;
	
	@Autowired
	private RManager rm;
	
	
	// 상세보기 ====================================================================================================================================================
	@RequestMapping("/detail.do")
	public String detail(Model model, String no, HttpSession session, HttpServletRequest request){
		
		try {

			
			VO vo = dao.Detail(Integer.parseInt(no));				// DAO의 상세보기 메소드 리턴값을 vo에 담기 
			
			// 워드클라우드 -----------------------------------------------------------------------------------------
			nm.naverData(vo.getSubject());
			rm.graph(Integer.parseInt(no));
			
			model.addAttribute("vo", vo);
			model.addAttribute("reply_list", reply_list);
			
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		
		return "/detail";
	}
```




## 화면 출력하기

- 저장한 워드클라우드 png파일 원하는 위치에서 출력하기
