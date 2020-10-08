# Collection Framework : 자료구조
- 라이브러리이기 때문에 표준화되어 있다.
- 데이터를 모아서 처리하게 쉽게 제공한다.

## 1. List
- 인터페이스이기 때문에 인터페이스를 구현하고 있는 클래스를 이용해서 메모리 할당을 한다.
- 순서가 있다.
- 중복된 데이터를 사용할 수 있다.<br>

(1) ArrayList <br>
(2) Vector <br>
(3) LinkedList <br>

## 2. Set
- 인터페이스이다.
- 순서가 없다
- 중복된 데이터를 사용할 수 없다.

(1) TreeSet <br>
(2) HashSet <br>


## 3. Map
- 키와 값을 나눠서 저장할 수 있는 공간이다.
- 키는 중복될 수 없다.
- 값은 중복될 수 있다.
- 클래스를 미리 메모리할당하고 키를 이용해서 할당된 주소를 찾아서 사용할 때 많이 사용한다.
- 웹에서 사용되는 request,response,session,cookie 들이 Map방식으로 저장된다.

(1) HashMap<br>
(2) HashTable<br>