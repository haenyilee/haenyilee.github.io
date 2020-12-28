---
sort: 2
---

# 4. java.io

> IO : Input(저장) , Outoput(읽기)

### I/O 클래스의 이름과 의미
- Stream으로 끝나는 클래스 : 바이트 단위로 입출력을 수행하는 클래스


|InputStream|1차|2차|
|-----------|------------|--------------------|
||FileInputStream|ObjectInputStream|
||StringBufferInputStream|InputStream|
|||DataInputStream|
|||BufferedInputStream|


|OutputStream|1차|2차|
|-----------|------------|--------------------|
||FileOutputStream|OutputStream|
|||DataOutputStream|
|||BufferedOutputStream|


OutputStream


- Reader / Writer로 끝나는 클래스 : 캐릭터 단위로 입출력을 수행하는 클래스
- File로 시작하는 클래스 : 하드디스크의 파일을 사용하는 클래스

Data로 시작하는 클래스 : 자바의 원시 자료형을 출력하기 위한 클래스

Buffered로 시작하는 클래스 : 시스템의 버퍼를 사용하는 클래스
- BufferedInputStream , BufferdOutputStream




- FileInputStream, FileReader
- FileOutputStream , FileWriter
- BufferReader , InputStreamReader
- ObjectInputStream , ObjectOutputStream
- BufferedInputStream , BufferdOutputStream

- 데이터 저장하는 방법 3가지 
1. 정형화된 데이터 : 구분O, 필요한 데이터O, 필요없는 데이터 X
2. 반정형화 데이터 : 구분O, 필요한 데이터O, 필요없는 데이터 O
3. 비정형화 데이터 : 구분X, 필요한 데이터O, 필요없는 데이터 O

[스트림보다 버퍼의 속도가 더 빠른 이유](https://jhnyang.tistory.com/92)
- 스트림 : 통로를 만들어줘서 한 글자씩 읽어오는 것(파일-응용프로그램)
- 버퍼 : 중간에 메모리 임시공간을 만들어서 파일을 임시공간에 옮겨서 응용프로그램으로 읽어오는 것 (파일-임시공간-응용프로그램)
  - inputstreamreader를 반드시 같이 써줘야 하는데, 그 이유는 1byte를 2byte로 변환시켜줘야 한글이 안깨지기 때문이다.

### 종류
- 메모리 입출력 : output, input
- 파일 입출력
- 네트워크 입출력

### 장단점
- 장점 : 정확한 전송이 가능함 => 스트림(데이터 통로) 이용하기 때문에 
- 단점 : 단방향(입력과 출력이 다름)

### 데이터 읽는 방식
- 1byte (일반파일) : 바이트 스트림
  - InputStream / OutputStream (인터페이스 : read,write)
  - 업로드, 다운로드
- 2byte (문자,한글) : 문자 스트림
  - Reader / Writer (인터페이스)
- 객체자체를 저장 : 직렬화
  - ObjectInputStream
  - ObjectOutputStream
- 속도 빠르게 하는 것 : Buffer
  - BufferInputStream
  - BufferOutputStream

(주의점)  
1. 반드시 ```import java.io.*``` 사용해야 함
2. 반드시 예외처리 해야함 ```CheckException```


### 파일 읽기
- FileInputStream (1byte) : 한글이 있는 경우에는 한글이 깨짐
- FileReader (2byte) : 한글 안깨짐
- 형식
```java
FileInputStream fis = new FileInputStream("절대경로명");
while(파일끝까지)
{
  출력
}
fis.close(); // finally => try~catch에 상관없이 수행
```
- 데이터 읽는 두 가지 방식
1) 한글자씩 읽기: FileInputStream, FileReader  
```int read()``` * 리턴형이 글자 번호 , A는 65로 읽어옴 , char로 형변환해야함

2) 한줄씩 읽기 : BuffedReader  
```readLine()```


### 파일 쓰기
> 데이터를 쓰기(저장) 방법
- FileOutputStream : 한글자씩 저장
- FileWriter : 한 번에 저장
- BufferedReader , file


### 프로그램 작성(파일)
1. 전체 파일을 읽어서 분리 => 메모리에 저장
FileReader
StringTokenizer, split
2. 메모리에서 제어
(참고)
- 파일 : 분리할 수 없음 (전체를 통으로 저장하기 때문에) 
- 오라클 : 분리되어 있는 상태  
  - 필요시마다 오라클에 연결 => 메소드안에서 처리함

