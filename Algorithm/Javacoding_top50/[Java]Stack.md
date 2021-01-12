# [Java]Stack

## What is Stack?

- 먼저 들어간 것이 나중에 나오는 자료구조이다.

- LIFO : Last In First Out, 후입선출

- 나중에 들어간 것이 먼저 나오기 때문에, 끝에서 부터 삭제하기엔 LinkedList보다 Array가 더 적합하다.

- Stack으로 구현한 대표적인 프로그램으로는 계산지, redo-undo 등이 있다.

## Stack의 클래스

![image](https://user-images.githubusercontent.com/66978721/104252485-89a46c00-54b5-11eb-9bec-f052e12d7b1b.png)

### push() : 추가

- Stack의 가장 위에 item을 추가한다.

- 추가한 item을 반환한다.

```java
public E push(E item) {
    addElement(item);

    return item;
}
```

### pop() : 삭제

- stack의 가장 위의 값을 삭제한다.

- 삭제한 값을 반환한다.

- 만약 stack이 비어있으면 EmptyStackException을 발생시킨다.

```java
public synchronized E pop() {
    E       obj;
    int     len = size();

    obj = peek();
    removeElementAt(len - 1);

    return obj;
}
```

### peek() : 가장 위의 값 들여다보기

- 삭제하지 않고, 가장 위의 값을 들여다보는 메소드이다.

- 들여다본 값을 반환한다.

- pop()과 마찬가지로 stack이 비어있으면 EmptyStackException을 발생시킨다.

```java
public synchronized E peek() {
    int     len = size();

    if (len == 0)
        throw new EmptyStackException();
    return elementAt(len - 1);
}
```

### empty() : 초기화

- stack을 비울 때 사용한다.

```java
public boolean empty() {
    return size() == 0;
}
```

### search() : 위치 반환

- 인자값으로 받은 데이터의 위치를 반환한다.

- index를 반환한는 것이 아니다.

```java
public synchronized int search(Object o) {
    int i = lastIndexOf(o);

    if (i >= 0) {
        return size() - i;
    }
    return -1;
}
```

### isEmpty() : 비어있는지 확인

```java
public boolean isEmpty() {

    return top == null && size == 0;
}
```


### size() : size를 반환

```java
public int size() {

    return size;
}
```


## 출처
- [Java 스택(Stack) 클래스 정리](https://velog.io/@lshjh4848/Java-%EC%8A%A4%ED%83%9DStack-%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%A0%95%EB%A6%AC)
- [자바로 Stack 구현하기](https://ktko.tistory.com/entry/%EC%9E%90%EB%B0%94%EB%A1%9C-Stack-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
- [Java script를 이용한 Stack 구현](https://velog.io/@kimkevin90/Java-script%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Stack-%EA%B5%AC%ED%98%84)
