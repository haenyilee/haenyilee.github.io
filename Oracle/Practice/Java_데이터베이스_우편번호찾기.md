---
sort: 3
---

# 우편 번호 검색하기

# ZipcodeVO

- VO : Value Object
- 데이터를 한번에 모아서 관리하기 위한 사용자 정의 데이터형(클래스가 아니라...)
- 데이터 컬럼명, 데이터형을 확인해서 만들면됨
  - ```SQL>DESC zipcode;```명령어 통해서 확인 

```JAVA
public class ZipcodeVO {
	private String zipcode;
	private String sido;
	private String gugun;
	private String dong;
	private String bunji;
}
```

- 주소를 묶어서 출력해주는 변수 address만들기
```JAVA
public class ZipcodeVO {
	private String zipcode;
	private String sido;
	private String gugun;
	private String dong;
	private String bunji;
	private String address;
    
        // 리턴 뒷부분을 바꿔서 반환값을 변경해줄 수 있음
	public String getAddress() {
		return sido+" "+gugun+" "+dong+" "+bunji;
	}
	/* 세터는 필요없음 : 값 넣어줄일 없다는 뜻
	public void setAddress(String address) {
		this.address = address;
	}
	*/

}
```



# ZipcodeDAO
- DAO : Data Access Object
- 오라클 연결을 위해 매번 시행해줘야하는 코드
- [비슷한 예시](https://dydwo15.tistory.com/entry/27-ArrayList-%ED%81%B4%EB%9E%98%EC%8A%A4?category=563410)
  
```java
public class ZipcodeDAO {
	
	// 연결
	private Connection conn;
	
	// 문장전송 -> SQL
	private PreparedStatement ps;
	
	// 연결 : 오라클 주소
	private final String URL="jdbc:oracle:thin:@localhost:1521:XE";
	
	// 드라이버 등록
	public ZipcodeDAO()
	{
		try {
			Class.forName("oracle:jdbc:driver:OracleDriver");
		} catch (Exception e) {
			e.getMessage();
		}
	}
	
	// 연결메서드
	public void getConnection()
	{
		try {
			conn = DriverManager.getConnection(URL,"hr","happy");
			
		} catch (Exception e) {}
	}
	
	// 닫기메서드
	public void disConnection()
	{
		try {
			if(ps!=null) ps.close();
			if(conn!=null) conn.close();
			// exit
			
		} catch (Exception e) {}
	}

        // 우편번호 찾기 메서드 : 검색한 데이터값 집어넣기
	public ArrayList<ZipcodeVO> postfind(String dong)
	{
		ArrayList<ZipcodeVO> list=new ArrayList<ZipcodeVO>();
		
		try {
			// 연결
			getConnection();
			// SQL문장 전송
			String sql = "SELECT * FROM zipcode "
					+"WHERE dong LIKE '%'||?||'%'"; // preparedStatement : ?
			ps=conn.prepareStatement(sql);
			ps.setString(1, dong);
			ResultSet rs= ps.executeQuery(); // 실행
			while(rs.next()) // next함수는 한줄 다 찾고 끝부분에 남겨진 커서를, 담줄 첨으로 옮겨줌
			{
				// 검색 결과 갯수만큼 메모리 할당해줘야 하니까 new를 통해 메모리 주소 생성
				ZipcodeVO vo = new ZipcodeVO();
				
				// 배열에 값 넣어주기
				vo.setZipcode(rs.getString(1));
				vo.setSido(rs.getString(2));
				vo.setGugun(rs.getString(3));
				vo.setDong(rs.getString(4));
				vo.setBunji(rs.getString(5));
				
				list.add(vo);
			}
			
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		finally {
			disConnection();
		}
		return list;
	}
}
```

- 드라이버 등록
  - ```Class.forName()```은 어떤 기능일까? 
    - JDBC 드라이버와 같이 인스턴스를 별도로 관리하지 않는 대부분의 클래스의 경우 정적 블록을 통해 생성하고 관리함
    - 따라서 Class.forName() 메소드를 호출하면 인스턴스 생성과 초기화가 이루어 짐 
    - (추가) Class.forName()은 JDBC 4.0 이후로는 메소드를 호출하지 않아도 자동으로 드라이버를 초기화함 [[참고]](https://kyun2.tistory.com/23)

```java
	// 연결
	private Connection conn;
	
	// 쿼리문장
	private PreparedStatement ps;
	
	// 오라클 주소에 연결
	private final String URL="jdbc:oracle:thin:@localhost:1521:XE";
	
	// 드라이버 등록
	public ZipcodeDAO()
	{
		try {
			Class.forName("oracle:jdbc:driver:OracleDriver");
		} catch (Exception e) {
			e.getMessage();
		}
	}
```




- 오라클 계정연결
```java
	// 연결메서드
	public void getConnection()
	{
		try {
			conn = DriverManager.getConnection(URL,"hr","happy");
			
		} catch (Exception e) {}
	}
```

- 오라클 계정연결 닫기
  - 닫기를 안하면 오라클 부하 심해짐

```java
	// 닫기메서드
	public void disConnection()
	{
		try {
			if(ps!=null) ps.close();
			if(conn!=null) conn.close();
			// exit
			
		} catch (Exception e) {}
	}
```



- 열고 닫기 기능 첨부한 postfind(String dong)메서드 생성
```java
// 우편번호 찾기
	public ArrayList<ZipcodeVO> postfind(String dong) 
        // 찾아질 데이터 갯수가 몇개인지 모르니까 배열 못씀 => 가변형인 ArrayList사용

	{
		ArrayList<ZipcodeVO> list=new ArrayList<ZipcodeVO>(); 
		
		try {
			// 연결
			getConnection();

			// SQL문장 전송
			String sql = "SELECT * FROM zipcode "
					+"WHERE dong LIKE '%'||?||'%'"; // preparedStatement : ?
			ps=conn.prepareStatement(sql);

			// sql비워뒀던 ? 부분에 값 집어넣어주는 과정
                        ps.setString(1, dong);

			// SQL문장 실행
			ResultSet rs= ps.executeQuery();

		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		finally {
			// 닫기
			disConnection();
		}
		return list;
	}
```

- 쿼리 작성
    - SQL문장 작성 시 띄어쓰기에 주의해야함
      - 대문자 나오면 띄어쓰기
      - ```String sql = "SELECT * FROM zipcode WHERE dong LIKE '%'||?||'%'";```
      - [LIKE뒷부분이 이해안가면?](https://github.com/haenyilee/Oracle_Basic/wiki/Oracle_SQL%EC%96%B8%EC%96%B4_DML_SELECT#where_%EC%97%B0%EC%82%B0%EC%9E%90--like--%EC%9C%A0%EC%82%AC%EB%AC%B8%EC%9E%90%EC%97%B4-%EC%B0%BE%EA%B8%B0)

```java
// SQL문장 작성
String sql = "SELECT * FROM zipcode WHERE dong LIKE '%'||?||'%'";
```



- 작성한 쿼리문장 conn에 담아주기
```java
ps=conn.prepareStatement(sql);
```

- 쿼리문장에서 비워둔 ```?```에 값 넣어주기
      - 1은 ```LIKE '%'||?||'%'```에서 첫번째 물음표를 의미함
      - ```?```자리에 ```private String dong;```을 넣어준다는 의미임

```java
ps.setString(1, dong);
```






- 실행요청 & 처리결과 리턴
      - [executeQuery : SQL의 수행메소드](https://codedragon.tistory.com/5961)
      - ResultSet은 각 줄을 컬럼마다 한칸씩 저장해두는 클래스임(참조변수(데이터형)로 이해하면됨)
      - 그래서 rs는 오라클 데이터가 한줄씩 담겨있는 메모리라고 볼 수 있음
      - 이 문장이 수행되기 전(값 채우기 전)까지는 오라클 문법 사용, 이후부터는 자바문법 사용
```java
ResultSet rs= ps.executeQuery();
``` 







- 데이터 한줄씩 읽기
    - next()
      - 오라클은 한줄 다 찾으면 그 row끝부분에 커서가 위치하게됨
      - 다음 row를 찾기 위해서는 커서가 그 다음줄 맨앞으로 옮겨져야함
      - next함수는 한줄 다 찾고 끝부분에 남겨진 커서를, 담줄 첨으로 움직여주는 역할을 함
      - 마지막 줄에서는 다음줄이 없어서 next()함수의 값이 false가 되므로 while문을 빠져나가 종료하게 됨

```java
while(rs.next())
{
}
```



- 각 row마다 메모리 할당
    - 검색 결과 갯수만큼 메모리 주소를 지정해줘야 하니까 new를 통해 메모리 주소 생성
    - 검색한 결과가 3개라면, 3개의 값을 저장해야 하니까 new 3번 시행해야함
    - 만약 클래스 생성 안했다면 메모리 저장공간 하나만 생김
    - 메모리 공간 하나에 while문이 돌아가게 되면 이전 값은 날라가고 새로 저장한 값만 남음
    - while문 다 돌고나서 마지막 값밖에 메모리에 남아있지 않게됨
    - 이 문제를 해결하기 위해서 Class를 만들어서 new를 통해 한 번씩 메모리 공간 만들어서 값을 넣어주는 것임

```java
while(rs.next())
{
	ZipcodeVO vo = new ZipcodeVO();
}
```



- 배열에 값 넣어주기
```java
while(rs.next())
{
    // 검색 결과 갯수만큼 메모리 주소를 지정해줘야 하니까 new를 통해 메모리 주소 생성
    ZipcodeVO vo = new ZipcodeVO();
				
    // 배열에 값 넣어주기
    vo.setZipcode(rs.getString(1));
    vo.setSido(rs.getString(2));
    vo.setGugun(rs.getString(3));
    vo.setDong(rs.getString(4));
    vo.setBunji(rs.getString(5));
				
    list.add(vo);
}
```




# PostMain

- tf를 누르면 동작하는 기능을 구현
```java
	@Override
	public void actionPerformed(ActionEvent e) {
		// tf를 누르면 동작하는 기능을 구현
		if(e.getSource()==tf)
		{
			// 입력이 안되었을때 처리
			
			// 입력이 되었을 때 처리
			
			// 얘만이 오라클 연결하는 애라서 얘를 통해서 입력값 받아와야함
			
			// 출력
		}
		
	}
```

- 입력이 안되었을 때 처리
```java

	// 입력이 안되었을때
	if(dong.length()<1)
	{
		JOptionPane.showMessageDialog(this,"동/읍/면을 입력하세요");
		return;
	}
```


- 입력이 되었을 때 처리
```java
	// 입력이 되었을 때 처리
		
	for(int i=model.getRowCount()-1;i>=0;i--)
	{
	}

```
- 테이블 지워주는 이유
        - 안지워주면 검색한 값이 맨 첨 목록 맨 밑에 추가되기 때문에
```java
	// 입력이 되었을 때 처리
		
	for(int i=model.getRowCount()-1;i>=0;i--)
	{
		// 테이블 지우기 : 안지워주면 검색한 값이 맨 첨 목록 맨 밑에 추가됨
		model.removeRow(i);
	}
```


- 오라클 데이터베이스 값 받아오기
        - ZipcodeDAO()만이 오라클 연결하는 애라서 얘를 통해서 입력값 받아와야함
        - 생성자 활용 : ```ZipcodeDAO dao = new ZipcodeDAO();```

```java
	// 입력이 되었을 때 처리
		
	for(int i=model.getRowCount()-1;i>=0;i--)
	{
		// 테이블 지우기 : 안지워주면 검색한 값이 맨 첨 목록 맨 밑에 추가됨
		model.removeRow(i);
	}
			
	// 얘만이 오라클 연결하는 애라서 얘를 통해서 입력값 받아와야함
	ZipcodeDAO dao = new ZipcodeDAO();
	ArrayList<ZipcodeVO> list = dao.postfind(dong);

```




- 검색된 내용과 일치하는 값 표에 출력해주기
  - tablemodel에 배열값을 add해줄때
     - ```,```로 연결된 값들은 각각 다른 컬럼에 추가됨
     - ```+```로 연결된 값들은 내용이 붙어서 같은 칸에 추가됨
  - vo.getAddress()는 주소를 묶어서 출력해주는 변수임
     - 원래는 ```String[] data = {vo.getZipcode(), vo.Sido+" "+vo.Gugun+" "+vo.Dong+" "+vo.Bunji);```
     - 간편하게 만들어주기 위해서 ```String[] data = {vo.getZipcode(), vo.getAddress());```로 변환
     - address는 [ZipcodeVO](https://github.com/haenyilee/JAVA_Basic/wiki/Java_%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4_%EC%9A%B0%ED%8E%B8%EB%B2%88%ED%98%B8%EC%B0%BE%EA%B8%B0#vovalue-object) 에서 생성함


```java
	// 출력
	for(ZipcodeVO vo : list)
	{
		String[] data = {
			vo.getZipcode(),
			vo.getAddress() // 주소를 묶어서 출력해주는 변수(vo에서 만들었음)
		};
		model.addRow(data);
	}
```

