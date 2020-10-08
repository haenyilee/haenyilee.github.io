# 지니 뮤직차트200 데이터 수집하기
- 사이트 주소 : https://www.genie.co.kr/chart/top200
- 외부라이브러리 : jsoup사용


# MusicVO.java
> 패키지 : com.sist.dao

## 1. 컬럼명 확인해서 변수?객체? 선언
```java
package com.sist.dao;

public class MusicVO {

    private int mno;
    private String title;
    private String singer;
    private String album;
    private String state;
    private int idcrement;
    private String key;
    private String poster;
}

```

## 2. GETTER/SETTER만들기(캡슐화)
```java
public class MusicVO {

    private int mno;
    private String title;
    private String singer;
    private String album;
    private String state;
    private int idcrement;
    private String key;
    private String poster;
	public int getMno() {
		return mno;
	}
	public void setMno(int mno) {
		this.mno = mno;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	public String getSinger() {
		return singer;
	}
	public void setSinger(String singer) {
		this.singer = singer;
	}
	public String getAlbum() {
		return album;
	}
	public void setAlbum(String album) {
		this.album = album;
	}
	public String getState() {
		return state;
	}
	public void setState(String state) {
		this.state = state;
	}
	public int getIdcrement() {
		return idcrement;
	}
	public void setIdcrement(int idcrement) {
		this.idcrement = idcrement;
	}
	public String getKey() {
		return key;
	}
	public void setKey(String key) {
		this.key = key;
	}
	public String getPoster() {
		return poster;
	}
	public void setPoster(String poster) {
		this.poster = poster;
	}
}
```


# MusicMain.java
## 1. 예외처리
- jsoup의 connect기능 사용하려면 예외처리 필수임

```java
public class MusicMain {

	public static void main(String[] args) {
		
		try {

		} catch (Exception e) {
			
		}

	}

}
```
## 2. 파싱하려는 웹주소 복사해서 url연결
- 페이지 갯수만큼 for문 돌리기
```java
public class MusicMain {

	public static void main(String[] args) {
		
		try {
			for(int i=1;i<=4;i++)
			{
			}
		} catch (Exception e) {
			
		}

	}

}
```

- 페이지 url연결해서 페이지소스 긁어오기
```java
public class MusicMain {

	public static void main(String[] args) {
		
		try {
			for(int i=1;i<=4;i++)
			{
				Document doc = Jsoup.connect("https://www.genie.co.kr/chart/top200?ditc=D&ymd=20200807&hh=09&rtm=Y&pg="+i).get();
			}
		} catch (Exception e) {
			
		}

	}

}
```

## 3. html에서 각 컬럼요소 추출하기
- html 태그 별로 구분해서 값 넣기
```java
public class MusicMain {

	public static void main(String[] args) {
	
		try {
			for(int i=1;i<=4;i++)
			{
				Document doc = Jsoup.connect("https://www.genie.co.kr/chart/top200?ditc=D&ymd=20200807&hh=09&rtm=Y&pg="+i).get();
				Elements title=doc.select("td.info a.title");
        			Elements singer=doc.select("td.info a.artist");
        			Elements album=doc.select("td.info a.albumtitle");
        			Elements poster=doc.select("a.cover img");  // <>안에 있는 정보
        			Elements temp=doc.select("span.rank");
			}
		} catch (Exception e) {			
		}
	}
}
```

## 3.1 element 갯수만큼 for문 돌려주기
```java
public class MusicMain {

	public static void main(String[] args) {
	
		try {
			for(int i=1;i<=4;i++)
			{
				Document doc = Jsoup.connect("https://www.genie.co.kr/chart/top200?ditc=D&ymd=20200807&hh=09&rtm=Y&pg="+i).get();
				Elements title=doc.select("td.info a.title");
        			Elements singer=doc.select("td.info a.artist");
        			Elements album=doc.select("td.info a.albumtitle");
        			Elements poster=doc.select("a.cover img");  // <>안에 있는 정보
        			Elements temp=doc.select("span.rank");

        		for(int j=0;j<title.size();j++)
        		{
        		}

			}
		} catch (Exception e) {			
		}
	}
}
```


## 3.2 파싱해온 요소들 잘 들어갔는지 확인하기
```java
public class MusicMain {

	public static void main(String[] args) {
	
		try {
			for(int i=1;i<=4;i++)
			{
				Document doc = Jsoup.connect("https://www.genie.co.kr/chart/top200?ditc=D&ymd=20200807&hh=09&rtm=Y&pg="+i).get();
				Elements title=doc.select("td.info a.title");
        			Elements singer=doc.select("td.info a.artist");
        			Elements album=doc.select("td.info a.albumtitle");
        			Elements poster=doc.select("a.cover img");  // <>안에 있는 정보
        			Elements temp=doc.select("span.rank");

        		for(int j=0;j<title.size();j++)
        		{
        			// html 정보 잘 긁혔는지 확인용
        			System.out.println("순위:"+k);
        			System.out.println("노래명:"+title.get(j).text());
        			System.out.println("가수명:"+singer.get(j).text());
        			System.out.println("앨범명:"+album.get(j).text());
        			System.out.println("포스터:"+poster.get(j).attr("src"));
        			System.out.println("상태등폭:"+temp.get(j).text());
        		}

			}
		} catch (Exception e) {			
		}
	}
}
```

## 3.3 temp데이터 가공하기 : 유지,new
  - temp에는 상태와 등락폭이 붙어서 출력되기 때문에 idcrement와 state 변수로 나눠서 데이터를 가공해줘야함
  - 유지, new일때는 등폭값이 0임
```java
public class MusicMain {

	public static void main(String[] args) {
	
		try {
			for(int i=1;i<=4;i++)
			{
				Document doc = Jsoup.connect("https://www.genie.co.kr/chart/top200?ditc=D&ymd=20200807&hh=09&rtm=Y&pg="+i).get();
				// 소스 잘 읽혔나 확인용 : System.out.println(doc);
				Elements title=doc.select("td.info a.title");
        			Elements singer=doc.select("td.info a.artist");
        			Elements album=doc.select("td.info a.albumtitle");
        			Elements poster=doc.select("a.cover img");  // <>안에 있는 정보
        			Elements temp=doc.select("span.rank");

        		for(int j=0;j<title.size();j++)
        		{
        			// html 정보 잘 긁혔는지 확인용
        			System.out.println("순위:"+k);
        			System.out.println("노래명:"+title.get(j).text());
        			System.out.println("가수명:"+singer.get(j).text());
        			System.out.println("앨범명:"+album.get(j).text());
        			System.out.println("포스터:"+poster.get(j).attr("src"));
        			// System.out.println("상태등폭:"+temp.get(j).text());

        			// 상태등폭이 붙어서 출력되니까 각 값을 잘라주기
        			String str=temp.get(j).text();
        			String idcrement="";
        			String state="";
        			if(str.equals("유지"))
        			{
        				idcrement="0";
        				state="유지";
        			}
        			else if(str.equals("new"))
        			{
        				idcrement="0";
        				state="new";
        			}
        			else
        			{
        				// 숫자빼고 다 지워라 => 숫자값만 가져오기
        				// 한글빼고 다 지워라 => 한글값만 가져오기

        			}
        			System.out.println("상태:"+state);
        			System.out.println("등폭:"+idcrement);
        			// System.out.println("동영상키 :"+youtubeKeyData(title.get(j).text()));
        			System.out.println("-------------------------------");
        		}

			}
		} catch (Exception e) {			
		}
	}
}
```


## 3.4 temp데이터 가공하기 : 상승,하강
- 상승,하강일때 등폭값의 자릿수가 달라지기때문에 결과값 replaceall 활용해서 나눠주기
- [[Java] 문자열 치환(Replace) 사용법 & 예제](https://coding-factory.tistory.com/128)
  - 숫자는 일의 자리, 십의 자리, 백의 자리에 따라 자릿수가 달라지기 때문에 ```substring```활용이 불가함
- 자주 쓰이는 정규식 패턴 [출처](https://highcode.tistory.com/6)
  - 숫자만 : ^[0-9]*$
  - 영문자만 : ^[a-zA-Z]*$
  - 한글만 : ^[가-힣]*$


```java
public class MusicMain {

	public static void main(String[] args) {
	
		try {
			for(int i=1;i<=4;i++)
			{
				Document doc = Jsoup.connect("https://www.genie.co.kr/chart/top200?ditc=D&ymd=20200807&hh=09&rtm=Y&pg="+i).get();
				// 소스 잘 읽혔나 확인용 : System.out.println(doc);
				Elements title=doc.select("td.info a.title");
        			Elements singer=doc.select("td.info a.artist");
        			Elements album=doc.select("td.info a.albumtitle");
        			Elements poster=doc.select("a.cover img");  // <>안에 있는 정보
        			Elements temp=doc.select("span.rank");

        		for(int j=0;j<title.size();j++)
        		{
        			// html 정보 잘 긁혔는지 확인용
        			System.out.println("순위:"+k);
        			System.out.println("노래명:"+title.get(j).text());
        			System.out.println("가수명:"+singer.get(j).text());
        			System.out.println("앨범명:"+album.get(j).text());
        			System.out.println("포스터:"+poster.get(j).attr("src"));
        			// System.out.println("상태등폭:"+temp.get(j).text());

        			// 상태등폭이 붙어서 출력되니까 각 값을 잘라주기
        			String str=temp.get(j).text();
        			String idcrement="";
        			String state="";
        			if(str.equals("유지"))
        			{
        				idcrement="0";
        				state="유지";
        			}
        			else if(str.equals("new"))
        			{
        				idcrement="0";
        				state="new";
        			}
        			else
        			{
        				// 숫자빼고 다 지워라 = 숫자값만 가져오기
        				idcrement=str.replaceAll("[^0-9]", "");
        				// 한글빼고 다 지워라 = 한글값만 가져오기
        				state=str.replaceAll("[^가-힣]", "");

        			}
        			System.out.println("상태:"+state);
        			System.out.println("등폭:"+idcrement);
        			// System.out.println("동영상키 :"+youtubeKeyData(title.get(j).text()));
        			System.out.println("-------------------------------");
        		}

			}
		} catch (Exception e) {			
		}
	}
}
```





## 4. 유튜브에서 음악 뮤직비디오 영상 키값 가져오기
- 유튜브에서 음악제목 검색하면 가장 처음 나오는 동영상의 키값을 가져오는 메소드임
```
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	// 유튜브 키값 가져오는 메소드 생성
		// 유튜브 웹사이트 연결
		// 영상 검색하면 나오는 첫 영상의 키값 추출하기
}
```


## 4.1 youtubeKeyData 메소드 만들기
- 변수는 위에서 추출해온 음악 제목 title로 넣기
- 영상 키값은 key라는 변수?객체?에 넣어줄 것임
- 고로 반환값은 key임

```
public class MusicMain {
	public static void main(String[] args) {        
	}

	// 유튜브 키값 가져오는 메소드
	public static String youtubeKeyData(String title)
	{
		String key="";
		return key;
	}

}
```


## 4.2 유튜브 웹사이트 연결하기
- Jsoup.connect를 사용하려면 예외처리 필수임
  - 상세 : Connection org.jsoup.Jsoup.connect(String url)

```
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	public static String youtubeKeyData(String title)
	{
		String key="";
		try {
		    // 유튜브 검색페이지와 커넥트
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return key;
	}

}
```

- 유튜브에서 제목(title) 검색하면 나오는 사이트와 연결
  - 연결할 주소가 https://www.youtube.com/results?search_query=다시+여름+바닷가 
  - ```=``` 뒷부분에 나오는 값들이 각 음악마다 달라지므로 지니뮤직에서 추출했던 제목을 넣어줘야함

```
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	public static String youtubeKeyData(String title)
	{
		String key="";
		try {
			Document doc = Jsoup.connect("https://www.youtube.com/results?search_query="+title).get();			
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return key;
	}

}
```

## 4.3 영상 검색한 페이지소스에서 키값 추출하기
1. 자바에서 정규식을 사용하기 위해서는 Pattern, Matcher 클래스 객체가 필요합니다. 
2. 문자열로 정의한 정규 표현식을 Pattern 객체로 만들기 위해 Pattern 클래스의 compile() 메소드를 사용합니다. 
3. 컴파일된 패턴은 Matcher 객체를 만드는 데 사용되며,  
   Matcher객체는 임의의 입력 문자열이 패턴에 부합되는지 여부를 판가름하는 기능을 가지고 있습니다. 
4. Pattern 객체들은 비상태 유지 객체들이기 때문에 여러 개의 Matcher 객체들이 공유할 수 있습니다.


## 4.3.1 페이지 소스에서 키값 추출하는 정규식을 패턴으로 만들기

  - 문자열로 정의한 정규 표현식을 Pattern 객체로 만들기 위해서는?
    - Pattern 클래스의 compile() 메소드를 사용해야함
    - compile은 Pattern 클래스 주요 메서드임
    - compile(String regex) : 주어진 정규표현식으로부터 패턴을 만든다.

```java
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	public static String youtubeKeyData(String title)
	{
		String key="";
		try {
			Document doc = Jsoup.connect("https://www.youtube.com/results?search_query="+title).get();
			
			// 정규표현식을 패턴으로 만들기
			Pattern p = Pattern.compile(String regex);

		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return key;
	}

}
```


## 4.3.2 문자열로 정의한 정규식 만들기
  - 소스 내에서 ```/watch?v=``` 뒤에 붙는 동영상 키값만 가져오는 정규식을 만들 것임
    - ```\\?``` : ```?```는 이미 사용중인 기호라서 앞에 \\을 붙여줘야함
    - ```[^가-힇]``` : 한글제외하고 다 가져와라
    - ```+``` : 글자수제한없이 가져와라
```java
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	public static String youtubeKeyData(String title)
	{
		String key="";
		try {
			Document doc = Jsoup.connect("https://www.youtube.com/results?search_query="+title).get();
			
			// 정규표현식을 패턴으로 만들기
			Pattern p = Pattern.compile("/watch\\?v=[^가-힇]+");

		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return key;
	}

}
```


## 4.3.3 패턴을 doc와 매치시키기
  - Pattern클래스의 메서드인 matcher를 이용해서 패턴을 doc와 매치시키기
  - Matcher객체 m은 임의의 입력 문자열이 패턴에 부합되는지 여부를 판가름하는 기능을 가짐
```java
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	public static String youtubeKeyData(String title)
	{
		String key="";
		try {
			Document doc = Jsoup.connect("https://www.youtube.com/results?search_query="+title).get();
			
			// 정규표현식을 패턴으로 만들기
			Pattern p = Pattern.compile("/watch\\?v=[^가-힇]+");
			// 패턴을 doc와 매치시킨다.
			Matcher m = p.matcher(doc.toString());

		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return key;
	}

}
```


## 4.3.4 find()와 group()을 통해 패턴에 맞는 값 1개씩 찾아내기

  - Pattern에 compile로 정규식(regEx)을 담고, Matcher에 타겟 스트링을 담고나서,
  - Matcher의 find() 함수를 쓰면 1번째 값을 찾아내고, true 혹은 false를 반환한다.
  - group() 을 쓰면 방금 find()함수를 통해 찾은 1번째 스트링이 튀어나온다.
  - 다시 find()를 쓰면 2번째 값을 찾고, group()을 쓰면 2번째 값이 튀어나오고... 이런 식이다.
  - 따라서 while문을 사용해 find()가 false가 될 때까지 루프를 돌려야 전체 값을 읽어올 수 있다.

```java
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	public static String youtubeKeyData(String title)
	{
		String key="";
		try {
			Document doc = Jsoup.connect("https://www.youtube.com/results?search_query="+title).get();
			
			// 정규표현식을 패턴으로 만들기
			Pattern p = Pattern.compile("/watch\\?v=[^가-힇]+");
			// 패턴을 doc와 매치시킨다.
			Matcher m = p.matcher(doc.toString());

			// 패턴에 맞는 값이 있으면 그 값을 추출해서 key객체에 담아주기
			while(m.find())
			{
				String str=m.group();
				key=str;
				break;
			}


		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return key;
	}
}
```


## 4.3.5 노래 제목 검색하면 나오는 첫번째 동영상의 키값만 가져오기
  - m.group()을 확인해보면 아래 값을 읽어온다는 것을 알 수 있음
```
/watch?v=ESKfHHtiSjs","webPageType":"WEB_PAGE_TYPE_WATCH","rootVe":3832}},"watchEndpoint": 
{"videoId":"ESKfHHtiSjs"}},"badges":[{"metadataBadgeRenderer":{"style":"BADGE_STYLE_TYPE_SIMPLE","label":"
```
  - 따라서 substring기능을 통해서 ```=``` 뒷부분부터 ```"```까지만 잘라와야 동영상 키값을 얻을 수 있음
    - substring의 시작 : ```str.indexOf("=")+1```
    - substring의 끝 : ```str.indexOf("\"")```

```java
public class MusicMain {
	public static void main(String[] args) {        
	}
	
	public static String youtubeKeyData(String title)
	{
		String key="";
		try {
			Document doc = Jsoup.connect("https://www.youtube.com/results?search_query="+title).get();
			
			// 정규표현식을 패턴으로 만들기
			Pattern p = Pattern.compile("/watch\\?v=[^가-힇]+");
			// 패턴을 doc와 매치시킨다.
			Matcher m = p.matcher(doc.toString());

			// 패턴에 맞는 값이 있으면 그 값을 추출해서 key객체에 담아주기
			while(m.find())
			{
				String str=m.group();
				// System.out.println(m.group());
				str=str.substring(str.indexOf("=")+1,str.indexOf("\""));

				key=str;
				break;
			}


		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return key;
	}
}
```



# MusicDAO

## 0. 개요
```java
public class MusicDAO {
	
	// 연결
	// 테이블 칼럼 순서대로 값 넣어주기
	// 드라이버 등록
	// 오라클 연결
	// 오라클 연결해제
	// 데이터 추가(기능)

}
```

## 1. 오라클 연결
```java
public class MusicDAO {
	
	// 연결
	private Connection conn;
	
	// 오라클에 전송할 쿼리
	private PreparedStatement ps;
	
	// 오라클에 연결하려면 url이 필요함
	private final String URL="jdbc:oracle:thin:@localhost:1521:XE";
	
	// 순서대로 값 넣어주기
	/*   mno NUMBER(3),
             title VARCHAR2(300), WHERE mno=1
             singer VARCHAR2(100),
             album VARCHAR2(200),
             poster VARCHAR2(1000),
             state CHAR(6), 
             idcrement NUMBER(3),
             key VARCHAR2(50)
        */
	
	// 드라이버 등록
	// 오라클 연결
	// 오라클 연결해제
	// 데이터 추가(기능)

}
```

## 1.1 오라클 연결
## 1.2 오라클에 전송할 쿼리
## 1.3 오라클에 연결할 url
## 1.4 드라이버 등록
## 1.5 오라클 연결 & 해제
- 가져올 값이 200개라서 오라클에 200번 연결해야 함
  - 지니뮤직에서 뮤직차트 200에 대한 정보를 받아올때 값 하나(노래정보 1개)를 받을때마다 row가 하나씩 추가되는 것이기 때문에 오라클을 200번 연결하게됨
- 한번 오라클 열어서 값을 받고 오라클 닫아준 뒤 닫아주지 않으면 오라클이 200개 연결되어서 시스템 과부하가 오게됨
  - 싱글턴패턴이 생기게된 이유가 이것임

## 2. 데이터 추가(기능) 
```java
public class MusicDAO {
	public void musicInsert(MusicVO vo)
	{
		
	}
}
```

## 2.1 지니뮤직 데이터 오라클 테이블에 INSERT하는 기능
- 매개변수가 8개인데 너무 많으니까 매개변수를 ```MusicVO vo```로 잡아주기
```
CREATE TABLE genie_music(
   mno NUMBER(3),
   title VARCHAR2(300),
   singer VARCHAR2(100),
   album VARCHAR2(200),
   poster VARCHAR2(1000),
   state CHAR(6), 
   idcrement NUMBER(3),
   key VARCHAR2(50)
);
```

## 2.2 오라클 연결 & 해제

```java
public class MusicDAO {
	public void musicInsert(MusicVO vo)
	{
		try {
			getConnection();
		} catch (Exception e) {
			System.out.println(e.getMessage());
		} finally {
			disConnection();
		}
		
	}
}
```

## 2.3 쿼리 작성 
- 테이블에 지니뮤직 페이지에서 파싱해온 정보를 컬럼마다 나눠서 넣어주기
  - 순서 잘 지켜야함
  - 데이터형 잘 맞춰서 넣어줘야함
```java
public class MusicDAO {
	public void musicInsert(MusicVO vo)
	{
		try {
			getConnection();
			// 쿼리 문장 작성

			// 쿼리문장 SQL연결해서 전송

			// 쿼리 문장 작성할때 ?로 비워뒀던 부분에 값 넣어주기

			// 실행
		} catch (Exception e) {
			System.out.println(e.getMessage());
		} finally {
			disConnection();
		}
		
	}
}
```

## 2.3.1 쿼리 문장 작성

## 2.3.2 쿼리문장 SQL연결해서 전송

## 2.3.3 쿼리 문장 작성할때 ?로 비워뒀던 부분에 값 넣어주기

## 2.3.4 실행
- 롤백안되니까 순서 잘 보고 저장하기

# [MusicMain 클래스] - [main 메서드]
## 1. MusicDAO 호출
MusicDAO dao = new MusicDAO();

## 2. MusicVO vo에 값 넣어주기
```java
public class MusicMain {

	public static void main(String[] args) {
		MusicDAO dao = new MusicDAO();

        		// 이부분에 지니뮤직 사이트에서 페이지 소스읽어서 값 읽오는 과정들 생략함(상동)

        			MusicVO vo = new MusicVO();
        			vo.setMno(k);
        			vo.setTitle(title.get(j).text());
        			vo.setSinger(singer.get(j).text());
        			vo.setAlbum(album.get(j).text());
        			vo.setPoster(poster.get(j).attr("src"));
        			vo.setState(state);
        			vo.setIdcrement(Integer.parseInt(idcrement));
        			vo.setKey(youtubeKeyData(title.get(j).text()));
        				
        			dao.musicInsert(vo); // 한줄씩 올라가게
        			
        			Thread.sleep(100);

        			k++;// 순위값


	}
	public static String youtubeKeyData(String title)
	{
		// 상세과정 생략(상동)
	}
}
```

- 정수는 정수로 변환해서 넣어주기
- 동영상은 youtubeKeyData(String title)함수에서 출력한 내용이 그대로 들어가게 


# Run SQL Command Line / Oracle SQL Developer
- 오라클에는 저장한 순서대로 데이터가 들어가기 때문에 ```ORDER BY mno```를 통해서 순위대로 정렬시켜서 값을 가져와야함
```SELECT mno,title FROM genie_music ORDER BY mno;```

- 정렬 안한 상태로 데이터 출력하면 순서가 뒤죽박죽인 것을 알 수 있음
```SELECT title FROM genie_music;```

- 싹쓰리의 곡들만 찾기
```SELECT * FROM genie_music WHERE singer LIKE '%싹쓰리%';```

# 빈 메모장
다른이름으로 저장
유니코드로 저장인가?
a.html

<iframe src="http://youtube.com/embed/ESKfHHtiSjs"></iframe>

- 크기조절
<iframe src="http://youtube.com/embed/ESKfHHtiSjs" width=500 height=500></iframe>