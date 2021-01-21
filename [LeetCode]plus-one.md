# [LeetCode] plus-one

## 문제
- [plus-one 문제 링크](https://leetcode.com/problems/plus-one/)

- 배열에 담긴 숫자 각각을 하나의 숫자로 생각해서 1을 더한 값을 배열 형태로 출력하라


## 해결방안


## 틀린답안

- [9,9,9] 일때 [10,0,0]을 반환한다.

```java
package PlusOne;

import java.util.Arrays;

public class PlusOne {
    public static void main(String[] args) {
        int[] digits = {9,9,9};
//        int[] digits = {0,0,9};
//        int[] digits = {0,0,1}; pass
//        int[] digits = {9,8,9};
//        int[] digits = {2,3,4}; pass
        PlusOne po = new PlusOne();
        po.plusOne(digits);
        System.out.println(Arrays.toString(digits));
    }
    public int[] plusOne(int[] digits) {
        int len = digits.length;
        digits[len-1] = digits[len-1]+1;

        if(digits[len-1]>10) {
            return digits;
        }

        for(int i = len-1; i>0; i--) {
            if(digits[i]>9) {
                carry(i, digits);
            }
        }


        if(digits[0]>9) {
            int[] newDigits = new int[len+1];
            newDigits[0] = 1;
            for(int i = 1; i<newDigits.length; i++) {
                newDigits[i] = 0;
            }
            //System.out.println(Arrays.toString(newDigits));
            return newDigits;
        }

        return digits;
    }
    public void carry(int index, int[] digits) {
        digits[index] = 0;
        digits[index-1]=digits[index-1]+1;
    }
}
```
