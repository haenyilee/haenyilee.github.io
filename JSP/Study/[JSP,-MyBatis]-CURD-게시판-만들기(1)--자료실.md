---
sort: 2
---

# 1. 환경설정 및 셋팅

## (1) 테이블 생성
```
CREATE TABLE databoard4(
   no NUMBER,
   name VARCHAR2(34) CONSTRAINT db4_name_nn NOT NULL,
   subject VARCHAR2(1000) CONSTRAINT db4_sub_nn NOT NULL,
   content CLOB CONSTRAINT db4_cont_nn NOT NULL,
    pwd VARCHAR2(10) CONSTRAINT db4_pwd_nn NOT NULL,
   regdate DATE DEFAULT SYSDATE,
   hit NUMBER DEFAULT 0,
   filename VARCHAR2(260),
   filesize NUMBER DEFAULT 0,
   CONSTRAINT db4_no_pk PRIMARY KEY(no)
);
```

```oracle
INSERT INTO databoard7(no,name,subject,content,pwd,filename)
VALUES(1,'홍길동','마이바티스 curd이용법','JSP include가 있는 경우 처리방법','1234',' ');COMMIT;
```

#### jsp만으로 만들게 되면?
- 소규모 웹사이트 제작밖에 못하게됨
- 이미 20년 지난 기법임
- 요즘은 프레임워크를 활용하는 추세임

## (2) cos.jar다운로드해서 추가
- 파일 업로드 라이브러리

## (3) config.xml 작성하기 
- src에 파일 생성
- 마이바티스 dtd복붙

- <트랜젝션 처리방법>
1. 자동처리 : 일반적으로 많이 사용 => JDBC 
2. 수동처리 : 프로그래머가 처리 => MANAGED

- <데이터소스 두가지 타입>
1. UNPOOLED : 요청할때마다 오라클 연결하고, 결과값을 가지고 오면 오라클 연결 해제를 시키는 것
  - 단점 : 연결하는 시간이 많이 소모되어 연결이 지연될 수 있다.
2. POOLED : 미리 DBCD(Connecion)을 만들어두어 연결하고 요청시마다 연결된 커넥션을 넘겨주는 방식
  - 장점 : 연결하는 시간이 소모되지 않는다.
  - 장점2 : 커넥션 객체를 제어할 수 있다.
  - 일반적으로 웹프로그램에서 많이 사용한다.

- mapper : SQL문장이 어디 있는지 확인하기 위함
  - XML에 저장되어 있는 데이터가 많으면 속도가 저하되기 때문에 분산해서 작성해야함
  - XML만 가지고 코딩하는 프레이워크였던 스트럿츠는 속도가 느려서 속도 빠른 스프링으로 전환되는 추세가 있었다.

## (4) db.properties 제작하기
- db접근 정보 미리 작성해두고 재사용하기 위함이다.
- ```변수=값```형식으로 이루어져 있음
- 마이바티스나 스프링은 기본 디폴트 폴더가 src이기 때문에 이곳에 파일을 넣어주면 자동으로 인식할 수 있음

## (5) VO 제작
- 오라클과 자바 데이터형 잘 매칭시켜야 한다.
  - BFILE , BLOB은 경로명만 주고 경로를 읽을 수 있게 한다.
  - Inputstream(IO)
- 데이터 보호 : 캡슐화 방식 , 변수 은닉화 , 외부 연결시에 getter/setter활용
```
Cf.<객체 지향의 3대 특성>
1. 캡슐화,은닉화
2. 상속 , 포함
3. 오버라이딩 , 오버로딩
```

## (6) mabatips플러그인 설치해서 mapper파일 제작
- 매번 dtd를 복사해와야아는 번거로움을 덜어준다.

- preparedstatement : 전송하는 기능
- parametertype : ?에 값을 채워주는 기능
  - ?가 한개일 경우, 일반 데이터형 주면된다.
  - ?가 여러개일 경우에는, java.util.Map(hashmap)을 쓸건지 DataBoardVO를 쓸건지 정해야한다.

- resultset : 결과값을 받는 기능(해쉬맵을 사용했을 경우에 필요하다)

- 각 태그는 반드시 한 개의 SQL문장만 작성해야한다.(CURD 중에서 한가지씩)

- 페이지를 나눠서 처리해야 하니까 인라인 뷰를 준다

- id는 중복이 되면 안되는 식별자이다.
  - 문자를 앞에서 시작해야 한다
  - 숫자는 뒤에 올 수 있다.

## (7) DAO
- xml파싱 후에 실행된 결과를 받는 위치이다.
- DAO에서 메소드 호출
- import java.io.*; // xml파일 읽는 용도
  - IO(INPUT,OUTPUT) : 데이터 입출력
    - 1byte씩 읽어들이는 것을 bytestream이라고 한다.(파일 업로드와 다운로드)
      - inputstream,outputstream
    - 2byte씩 읽어들이는 것을 문자 스트림이라고 한다.
      - ~reader , ~writer

- xml 파싱해서 읽은데이터를 저장해야 하는데 그 기능을 sqlsessionfactory가 한다.
- 초기화를 해서 xml읽기가 자동으로 셋팅되게 static블록을 설정한다.
- resources를 사용해서 config.xml에 있는 모든 xml문장을 읽어서 reader에 저장한다.
- xml을 파싱하면 마이바티스 라이브러리가 읽어간다.
- 태그를 한개씩 읽어와서 필요데이터를 저장하는 방식을 sax방식이라고 한다.
```
JAXP : 1. DOM(수정,추가,삭제 가능/메모리에 저장하기 때문에 속도 느림), 2. SAX(읽기만해서 속도가 빠름)
JAXB : 빅데이터, 데이터값을 자바에 채운다 , binding
```

## (8) main.jsp에서 화면 레이아웃 설정하기
- nav.jsp 와 login.jsp를 제작하여 main.jsp에 include한다. 




# 2. 글목록,페이지 분할 기능 만들기

## (1) mapper에서 저장된 게시글 불러오고, 페이지 나누는 sql문장 작성하기

## (2) DAO에서 boardListData, boardTotalPage 클래스 제작
- 커넥션 사용한 다음에 반드시 반환해야 한다. 이때 sql세션이 커넥션과 동일한 기능을 수행한다.
- 컴파일 예외 처리는 없다. 하지만 초반에는 에러처리 위해서 반드시 예외처리해주는 것이 좋다.


## (3) list.jsp에서 자료실 리스트 화면 출력하기
- 자바 부분 코딩 : 사용자 요청한 페이지 값 받아서 list에 담긴 게시글 데이터 받기
- 화면 출력 : 각 페이지에 해당하는 VO 값 뿌리기


## (4) main.jsp에서 화면 변경
- 자바 부분 코딩 : 사용자가 요청한 mode값을 받아서 해당 화면을 띄워준다
  - 최초 페이지의 경우 사용자 입력값이 없으니 디폴트 페이지 설정하기
  - mode 1일 경우, ```../board/list.jsp```로 이동





# 3. 새글 작성하는 기능 추가하기

## (1) insert.jsp 제작하기
- 파일 업로드는 프로토콜이 달라서 form태그에 enctype을 주어야 한다.
  - 프로토콜은 범용적인 약속,규약이다.
  - 파일 전송할 때는 20번 통로를 써야한다...등이 바로 프로토콜이다.
  - 하지만 번호를 외울 수는 없으니까 명칭을 다 준것임
- 글 작성을 완료하면 바로 list.jsp로 넘어가면 안된다.
  - 글작성 완료 버튼을 눌러야만 db에 데이터를 넘겨주도록 설정해야하기 때문에 ok.jsp가 필요하다.
  - 이때, form태그를 사용한다.
```
<form method="post" action="../board/insert_ok.jsp" enctype="multipart/form-data">
```

## (2) insert_ok.jsp 만들기
- 글작성 완료하면 글목록으로 돌아가야한다 : list.jsp(mode=1)로 화면 전환
  - 이때 사용하는 게 sendredirect다.

- 글을 추가할때 파일 형식은 따로 처리해줘야 한다
- 파일 업로드 규칙 및 경로 작성하기
- new defaultfilrenamepolicy는 같은 파일명이 업로드될 시, <br>
뒤에 1,2.. 등의 숫자를 추가하여 이름 바꿔서 업로드 가능하도록 해주는 기능을 가지고 있다.

## (3) 작성한 내용이 디비에 쌓이도록 mapper파일에 sql문장 추가하기
- 받을 값이 없으면 resulttype필요없다

- Element : selectKey
  - 마이바티스에서 제공하는 기능으로 자동증가값을 설정하여 값을 INSERT할 때 주로 사용한다.
  - 참고 : [selectKey](https://yookeun.github.io/java/2014/07/11/mybatis-selectkey/)
```
<insert id="boardInsert" parameterType="DataBoardVO">
    <!-- Sequence 
         SELECT NVL(MAX(no)+1,1) FROM databoard을 먼저 수행한 후에
                 결과값을 no에 받아서 
         INSERT 문장에 추가
    -->
    <selectKey keyProperty="no" resultType="int" order="BEFORE">
      SELECT NVL(MAX(no)+1,1) FROM databoard4
    </selectKey>
    INSERT INTO databoard4 VALUES(
      #{no},
      #{name},
      #{subject},
      #{content},
      #{pwd},
      SYSDATE,
      0,
      #{filename},
      #{filesize}
    )
</insert>
```

## (4) sql문장을 실행하도록 dao에 내용 추가하기
- 작성한 데이터를 넘겨줄 뿐 받아올 값이 없으니까 void이다.
- 오토커밋 안되니까 오토커밋 설정하거나 session.commit()추가해야한다 
  - session.commit : 각 문장마다 커밋을 따로 날려줄 때 사용한다.
  - ssf.session(true) : 오토커밋을 설정할 때 사용한다.
- insert , update , delete는 데이터가 변경되는거니까 반드시 커밋을 날려줘야한다.


## (5) insert_ok.jsp에서 boardInsert dao호출하기 
```
//데이터베이스 연결하기 : dao호출해서 insert요청하는데 sql문장은 mapper에 있다.
DataBoardDAO.boardInsert(vo);
```
## (6) main.jsp에 include하기
- mode=2일때 insert.jsp로 화면이 넘어가도록 설정한다.



# 4. 내용 보기 기능 추가

## mapper에서 내용 보기 sql문장 작성하기
- 조회수 증가시키는 문장 작성
- 실제 데이터 읽는 문장 작성
- 중복된문장 있는 경우 재사용하는 문장 작성하면 된다.

# (중간부분 날라감)



# 5. 다운로드 기능 추가
## (1) detail.jsp에서 파일 다운로드 기능 추가

## (2) download.jsp만들기
- list.jsp에서 제목클릭하면 detail.jsp로 넘어가고 거기서 파일 다운로드하는 기능임
- 화면 출력 기능이 아니라 처리만 담당함



# 6. 검색해서 찾는 기능 추가하기
- 페이지는 나누지 않음

## (1) list.jsp에서 찾기 기능 추가
- 검색하는 부분에 form태그 주기
- post방식이니까 ```input type=hidden```으로 주기
    - post방식은 내부 네트워크를 이용해서 숨겨서 전송하는 방식임
    - Get방식은 url뒤에 값을 노출하는 방식임
    - ```데이터를 받을 파일명?변수명=값``` 형식으로 넘겨준다
    - 값이 여러개라면 ```변수명=값&변수명=값&변수명=값```

## (2) find.jsp 생성하기
- 모양만 잡기 (list.jsp와 비슷)

## (3) main.jsp에서 find.jsp페이지 include하기
- mode 4번과 find.jsp가 연동됨

## (4) list.jsp에서 버튼 클릭 과정 처리
- 찾기 버튼 클릭하면 자바스크립트로 처리하기
  - var데이터형 주면 알아서 인식함
- submit을 주게 되면 값이 입력 안되어도 검색이 가능하기 때문에 <br>
buttom으로 처리한 뒤, 자바스크립트로 기능을 설정해야함
```
<script type="text/javascript">
function send()
{
	var f=document.frm;
	if(f.ss.value=="")
	{
		f.ss.focus();
		return;
	}
	f.submit();
		
}
</script>
```

## (5) find.jsp에서 검색 기능 처리하기
- 자바로 사용자 입력값 받아두기
```
<%
     request.setCharacterEncoding("UTF-8");//utf-8
     String fd=request.getParameter("fd");
     String ss=request.getParameter("ss");
     System.out.println("fd:"+fd);
     System.out.println("ss:"+ss);
%>
```

## (6) mapper에서 검색한 내용 가지고 오는 SQL문장 작성하기
- ${} 와 #{}의 차이점은?
  - ${} : 컬럼명, 테이블명
  - #{} : 일반 데이터
```
<select id="boardFindData" resultType="DataBoardVO" parameterType="hashmap">
		SELECT no,subject,name,TO_CHAR(regdate,'YYYY-MM-DD') as dbday, hit
		FROM databoard
		WHERE ${fd} LIKE '%'||#{ss}||'%'
</select>
<select id="boardFindCount" resultType="int" parameterType="hashmap">
		SELECT COUNT(*)
		FROM databoard4
		WHERE ${fd} LIKE '%'||#{ss}||'%'
	</select>
```

## (7) dao에서 sql문장 실행 후 결과값 받기
- findcount, finddata

## (8) find.jsp에서 dao에서 받아둔 결과값 출력하기
- 자바에서 검색 결과값, 갯수 받기
- 결과값이 0개일때와 그 외의 경우로 나눠서 코딩해야함


# 삭제 기능 추가하기 
## delete.jsp 추가
- 모양만 잡기

## main.jsp에 추가
- mode5번에 delete.jsp 추가하기

## detail.jsp에 추가
- 삭제 버튼 누르면 넘어가는 링크가 delete.jsp가 되도록 추가하기
```jsp
<a href="../main/main.jsp?mode=5" class="btn btn-xs btn-success">삭제</a>
```
- 특정 글번호의 글만 삭제하도록 no추가
```
<a href="../main/main.jsp?mode=5&no=<%=vo.getNo() %>" class="btn btn-xs btn-success">삭제</a>
```

## delete.jsp
- 특정 클번호 no받아오기
```
<%
	String no = request.getParameter("no");

%>
```
- 사용자가 입력한 비밀번호 출력은 하지 않고, 내부적으로 데이터만 전송하도록 한다.(hidden사용)
```
비밀번호:<input type=password name=pwd size=15 class="input-sm">
				<input type=hidden name=no value=<%=no %>>
```
- `<form method=post action="../board/delete_ok.jsp">`

## delete_ok.jsp만들기
- detail.jsp에서 게시물 번호와 비밀번호 데이터를 전송하므로, 이 두 값을 받아줘야 한다.

## mapper에서 db와 게시물 번호, 비밀번호 처리하는 sql문장 작성하기
- 쿼리 문장은 3개가 필요함

## dao에서 삭제하기 기능추가
- 필요한 클래스는 2개임
  - 한 개는 비밀번호가 일치하는지 확인하는 것
  - 다른 한 개는 삭제요청한 게시글 데이터 delete하는 것

## delete_ok.jsp
- DAO연결해서 SQL문장 실행하기
- 업로드 되었던 파일 삭제하기
  - 비밀번호가 맞는지 먼저 검증해야함
  - listfiles : 파일 정보 가져오기


# 수정하기 기능 추가
(1) 사용자가 수정 원하는 글의 기존 데이터 받아오기
(2) 사용자가 수정한 데이터 받아서 처리하기

## update.jsp 만들기
- 모양만 잡기 

## main.jsp에 update.jsp include하기
- mode6번을 update.jsp와 연동하기

## detail.jsp의 수정하기 버튼과 연동하기
- 수정하기 버튼이 클릭된 글의 no값 넘겨주기
```
<a href="../main/main.jsp?mode=6&no=<%=vo.getNo() %>" 
class="btn btn-xs btn-primary">수정</a>       
```

## update.jsp
- detail.jsp에서 넘겨준 게시물 번호(no)값 받기
- 다른 jsp에서 데이터를 보내면 항상 request를 통해서 값을 받아온다.
- 오라클에서 게시물 번호에 해당되는 데이터 읽어온다.
- 읽어온 데이터를 jsp가 받아서 화면에 출력해야 함

## dao에서 db데이터 읽기
- mapper를 보면 필요한 sql문장이 이미 만들어져있음
  - id=boardDetailData를 사용하면 됨

## update.jsp
- DAO에서 받은 결과값 뿌리기
  - detail.jsp와 똑같은 데이터를 받아둬야 함
- 첨부파일 수정은 어려우니까 주석 걸어서 처리하지 않기
  - form태그에 enctype도 지워주기

- update_ok.jsp에 no값 보내주기
```jsp
<input type=hidden name=no value=<%=no %>>
```

## update_ok.jsp만들기
- 화면 출력 기능은 없고, 데이터 처리해주고 화면 이동해주는 기능만 있음
- update.jsp에서 보내준 no 디코딩해주기
  - 전송할때는 인코딩해야함
```
request.setCharacterEncoding("utf-8");
```
- 사용자가 수정한 데이터 받아서 vo에 값 채우기
```
<%
String name=request.getParameter("name");
DataBoardVO vo=new DataBoardVO();
vo.setName(name);
%>
```


## mapper에서 sql문장 작성하기
```
<update id="boardUpdate" parameterType="DataBoardVO">
    UPDATE databoard SET
    name=#{name},
    subject=#{subject},
    content=#{content}
    <include refid="where-no"/><!-- WHERE no=#{no} -->
</update>
```
## DAO에서 sql문장 실행하기
#### boardUpdate
- 비밀번호 일치 여부 체크하기

## update_ok.jsp
- dao에서 비밀번호 확인한 결과 받아서 처리하기
- 비밀번호 맞으면 처리해서 detail.jsp로 보내기, 틀리면 처리x
```console
<%
boolean bCheck=DataBoardDAO.boardUpdate(vo)
     if(bCheck==true)
     {
    	 response.sendRedirect("../main/main.jsp?mode=3&no="+no);// detail로 이동 
     }
     else
     {
%>
         <script>
         alert("비밀번호가 틀립니다");
         history.back();
         </script>
<%
     }
%>
```

