# [LeetCode] unique-email-addresses

## 문제

- [unique-email-addresses문제 링크](https://leetcode.com/problems/unique-email-addresses/)

- 이메일 ID 부분에서 `.` 과 `+`뒷부분을 제거한 뒤, 중복 제외한 이메일 갯수를 구하여라

## 해결방법

- 중복을 제외해야 하니 담을 그릇을 SET으로 정한다.

- split, replaceAll, substring을 적절히 활용해 조건을 해결한다.

```warning
- `.` 은 정규표현식에서 사용되는 메타문자이다.

- 이런 특수문자를 문자 그대로 활용하기 위해선 이스케이프 문자 `\\`를 붙여야 한다.

- 따라서 `.`은 `\\.`으로 작성해주어야 한다.
```

## 코드

```java

// 문제 : https://leetcode.com/problems/unique-email-addresses/

import java.util.HashSet;
import java.util.Set;

public class T11_UniqueEmailAddress {
    public static void main(String[] args) {
        String[] emails =
                {"test.email+alex@leetcode.com",
                        "test.e.+mail+bob.cathy@leetcode.com",
                        "teste.+mail+bob.cathy@leetcode.com",
                        "testemail+david@lee.tcode.com"};

        T11_UniqueEmailAddress t11 = new T11_UniqueEmailAddress();
        System.out.println(t11.numUniqueEmails(emails));
    }
    public int numUniqueEmails(String[] emails) {
        String [] temps= null;

        // 중복값 제거하고 담을 그릇 : Set
        Set<String> set = new HashSet<>();

        for(String s:emails) {
            // @로 분리하기
            temps = s.split("@");
            String local = temps[0];
            String domain = temps[1];

            // locals에 . 없애기
            local = local.replaceAll("\\.","");

            // locals에서 + 뒤는 자르기
            if(local.contains("+")) {
                local = local.substring(0,local.indexOf('+'));
            }

            // local과 domain 합치기
            s = local + "@" + domain;
            System.out.println(s);

            // set에 s 담기
            set.add(s);
        }
        // set에 담겨진 갯수 반환하기
        return set.size();
    }
}
```
