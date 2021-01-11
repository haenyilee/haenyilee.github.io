# [java] 입출력

## 정수를 입력받을 때

- 공백 또는 다음줄에 입력받을 때 모두 `nextInt()`를 사용한다.

## 빠른 입출력

- 본격적으로 for문 문제를 풀기 전에 주의해야 할 점이 있다. 입출력 방식이 느리면 여러 줄을 입력받거나 출력할 때 시간초과가 날 수 있다는 점이다.

- Java를 사용하고 있다면, Scanner와 System.out.println 대신 BufferedReader와 BufferedWriter를 사용할 수 있다. BufferedWriter.flush는 맨 마지막에 한 번만 하면 된다.

- Java는 BufferedWriter 외에도, StringBuilder로 출력을 모아 놓았다가 그 String을 System.out.println하는 방법도 있습니다.

- Python을 사용하고 있다면, input 대신 sys.stdin.readline을 사용할 수 있다. 단, 이때는 맨 끝의 개행문자까지 같이 입력받기 때문에 문자열을 저장하고 싶을 경우 .rstrip()을 추가로 해 주는 것이 좋다.

- 또한 입력과 출력 스트림은 별개이므로, 테스트케이스를 전부 입력받아서 저장한 뒤 전부 출력할 필요는 없다. 테스트케이스를 하나 받은 뒤 하나 출력해도 된다.


## BufferedReader와 BufferedWriter
- Scanner와 System.out.println보다 빠르다.

- Buffer : 데이터를 한 곳에서 다른 한 곳으로 전송하는 동안 일시적으로 그 데이터를 보관하는 임시 메모리 영역

- BufferedReader 
  - Scanner와 같은 기능을 한다.
  - 엔터만 경계로 인식한다
  - 받은 데이터가 String으로 고정된다.

- BufferedWriter
  - System.out.println과 같은 기능을 한다.
  - 출력과 개행을 동시에 해주지 않는다.

- BufferedWriter.flush
  - 버퍼를 비우는 동작이다.
  - 버퍼에 남아있는 데이터를 출력시킨다.
  
  
### BufferedReader

### BufferedWriter

## StringBuilder
