# 20201106-MyBatis Annotaion

## 여러개의 파일 다운로드 받는 법

- filename, filesize,filecount 매퍼에 추가
- file받을 리스트 배열 생성
- detail.jsp 
- 업로드 파일 갯수가 일정하지 않을 때에는 각 파일마다 컬럼을 나누지 않고, 구분자로 이어서 저장한다.


## jstl로 for문 2번 돌리기

- 두 배열의 첨자를 활용한다.
- varStatus가 fList의 인덱스 번호임
- fList.get(0) => sList.get(0) = ${sList[s.index]}


## 파일 다운로드 하기

### BoardController.java
- throw문장을 써도 호출할 떄 문제가 없다
  - 왜냐하면 컨트롤러가 예외처리를 하고 들어오기 때문ㅇ?
  
- 헤더 : 데이터 전송 전에 보내는 내용 : 다운로드 창을 보여준다 (파일명 , 파일크기)


- response : 응답 ()



### mapper
- 
