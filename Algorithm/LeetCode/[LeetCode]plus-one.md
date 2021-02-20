# [LeetCode] Plus One

## 문제

- [plus-one 문제링크](https://leetcode.com/problems/plus-one/)

- 마지막 자리의 수에서 1을 더한 값을 배열로 리턴하는 문제이다. 

## 풀이

- 1을 더했을 때, 각 자리의 숫자가 10을 넘는지 확인해야 한다.

- 10으로 나누었을 때, 나머지가 0인지 확인한다.

- 나머지가 0일 때는 그 앞자리수에 1을 더해줘야 한다.

- 첫번째 자리수까지 도달했을 때도 10으로 나누었을 때 나머지가 0이라면, 배열의 크기를 늘려준 뒤 첫 번째 자리에 1을 대입한다.

## 코드

```java
import java.util.Arrays;

public class T10_PlusOne {
    public static void main(String[] args) {
//        int[] digits = {1,2,3};
//        int[] digits = {9,9,9};
//        int[] digits = {1,9,9};
//        int[] digits = {1,4,9};
        int[] digits = {0,0,1};

        T10_PlusOne po = new T10_PlusOne();
        int[] result = po.plusOne(digits);
        System.out.println(Arrays.toString(result));
    }
    public int[] plusOne(int[] digits) {
        int index = digits.length - 1;
        int carry = 1;

        // index<0 || carry=0일 때 탈출
        while (index >= 0 && carry > 0) {
            digits[index] = (digits[index]+1) % 10;

            // 1을 더해서 10이 될 때
            if(digits[index] == 0) {
                carry = 1;
            }
            // 1을 더해서 10 이하일 때
            else {
                carry = 0;
            }
            index--;
        }

        if(carry == 1) {
            digits = new int[digits.length+1]; // 전부 0으로 초기화
            digits[0] = 1;
        }
        return digits;
    }
}
```

## 출처

- [정말 쉽게 풀어보는 코딩 테스트 top 기본 문제 (with 자바)](https://www.inflearn.com/course/%EC%BD%94%EB%94%A9%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%9E%90%EB%B0%94#)

