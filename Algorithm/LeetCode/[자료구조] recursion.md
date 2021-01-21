# [자료구조] 재귀함수(Recursion)

## 1. 재귀함수란?

- 자신의 함수 내부에서 자기 자신을 스스로 호출함으로써 재귀적으로 문제를 해결하는 함수이다.

- 재귀함수는 아주 간격하고 직관적인 코드로 문제를 해결할 수 있게 해준다.

- 반면 때에 따라서는 심각한 비효율성을 초래할 수 있기 때문에 유의가 필요하다.

- 반복함수는 for, while을 활용한 함수이다.

## 팩토리얼 구현하기

- 반복함수로 구현하기

```java
public static int factorial(int  number) {
    int sum = 1;
    for(int i = 2; i<= number; i++) {
        sum *= i;
    }
    return sum;
}
```

- 재귀함수로 구현하기

```java
public static int factorial(int  number) {
    if(number == 1)
        return 1;
    else
        return number * factorial(number - 1);
    // 5! = 5 * 4!
    // 5! = 5 * 4 * 3!
    // 5! = 5 * 4 * 3 * 2!
    // 5! = 5 * 4 * 3 * 2 * 1
}
```

## 피보나치 수열 구현하기

- 반복함수로 구현하기

```java
public static int fibonacci(int number) {
    int one = 1;
    int two = 1;
    int result = -1;
    if(number == 1)
    {
        return one;
    }
    else if(number == 2)
    {
        return two;
    }
    else
    {
        for(int i = 2; i < number; i++)
        {
            result = one + two;
            one = two;
            two = result;
        }
    }
    return result;
}
```
    
- 재귀함수로 구현하기

```java
public static int fibonacci(int number) {
    if(number == 1)
        return 1;
    else if(number == 2)
        return 1;
    else
    {
        return fibonacci(number - 1) + fibonacci(number - 2);
    }
}
```

- 재귀함수의 단점

![image](https://user-images.githubusercontent.com/66978721/104862940-e00f2000-5977-11eb-91bc-438c6326bc75.png)

위처럼 중복처리되는 경우가 많고, 한 단계 증가할 때마다 2배씩 증가하는 것을 볼 수 있다.
