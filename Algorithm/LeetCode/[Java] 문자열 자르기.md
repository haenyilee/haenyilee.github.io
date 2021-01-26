# [Java] 문자열 자르기


## SubString

![image](https://user-images.githubusercontent.com/66978721/105834079-60cfbb00-600d-11eb-8e21-355c60f22d00.png)

```java
public String substring(int beginIndex, int endIndex) {
        if (beginIndex < 0) {
            throw new StringIndexOutOfBoundsException(beginIndex);
        }
        if (endIndex > value.length) {
            throw new StringIndexOutOfBoundsException(endIndex);
        }
        int subLen = endIndex - beginIndex;
        if (subLen < 0) {
            throw new StringIndexOutOfBoundsException(subLen);
        }
        return ((beginIndex == 0) && (endIndex == value.length)) ? this
                : new String(value, beginIndex, subLen);
    }
```

### 기본 예제

```java
String str = "ABCDEFG"; //대상 문자열
/*A=0 B=1 C=2 D=3 E=4 F=5 G=6의 index를 가진다.*/
		
str.substring(3); 
/*substring(시작위치) 결과값 = DEFG*/

str.substring(3, 6); 
/*substring(시작위치,끝위치) 결과값 = DEF*/
```

### 마지막 3글자 자르기

```java
String str = "ABCDEFG"; 
String result = str.substring(str.length()-3, str.length());
System.out.println(result)
 //결과값EFG
```

### 특정문자 이후의 문자열 제거하기

```java
String str = "ABCD/DEFGH";
String result = str.substring(str.lastIndexOf("/")+1);
System.out.println(result); 
//결과값 DEFGH
```

### 특정단어(부분)만 자르기

```java
String str = "바나나 : 1000원, 사과 : 2000원, 배 : 3000원";
String target = "사과";
int target_num = str.indexOf(target); 
String result; result = str.substring(target_num,(str.substring(target_num).indexOf("원")+target_num));
System.out.println(result+"원"); 
//결과값 : 사과 : 2000원
```

## Split

![image](https://user-images.githubusercontent.com/66978721/105833525-a770e580-600c-11eb-9271-3b2366cc0f54.png)


```java
public String[] split(String regex, int limit) {
        /* fastpath if the regex is a
         (1)one-char String and this character is not one of the
            RegEx's meta characters ".$|()[{^?*+\\", or
         (2)two-char String and the first char is the backslash and
            the second is not the ascii digit or ascii letter.
         */
        char ch = 0;
        if (((regex.value.length == 1 &&
             ".$|()[{^?*+\\".indexOf(ch = regex.charAt(0)) == -1) ||
             (regex.length() == 2 &&
              regex.charAt(0) == '\\' &&
              (((ch = regex.charAt(1))-'0')|('9'-ch)) < 0 &&
              ((ch-'a')|('z'-ch)) < 0 &&
              ((ch-'A')|('Z'-ch)) < 0)) &&
            (ch < Character.MIN_HIGH_SURROGATE ||
             ch > Character.MAX_LOW_SURROGATE))
        {
            int off = 0;
            int next = 0;
            boolean limited = limit > 0;
            ArrayList<String> list = new ArrayList<>();
            while ((next = indexOf(ch, off)) != -1) {
                if (!limited || list.size() < limit - 1) {
                    list.add(substring(off, next));
                    off = next + 1;
                } else {    // last one
                    //assert (list.size() == limit - 1);
                    list.add(substring(off, value.length));
                    off = value.length;
                    break;
                }
            }
            // If no match was found, return this
            if (off == 0)
                return new String[]{this};

            // Add remaining segment
            if (!limited || list.size() < limit)
                list.add(substring(off, value.length));

            // Construct result
            int resultSize = list.size();
            if (limit == 0) {
                while (resultSize > 0 && list.get(resultSize - 1).length() == 0) {
                    resultSize--;
                }
            }
            String[] result = new String[resultSize];
            return list.subList(0, resultSize).toArray(result);
        }
        return Pattern.compile(regex).split(this, limit);
    }
```

### 특정문자로 자르기

```java
String str = "";

for(int i=0;i<5;i++) {
str += i+"#";
}
		
String[] array = str.split("#");
		
for(int i=0;i<array.length;i++) {
System.out.println(array[i]);
}

//결과값 
//array[0] = 1
//array[1] = 2
//array[2] = 3
//array[3] = 4
```


### 쉼표(,)로 문자열 잘라서 배열에 넣기

```java
String str = "A,B,C,D";
String[] array = str.split(",");
		    
//출력				
for(int i=0;i<array.length;i++) {
System.out.println(array[i]);
}
		  
//결과값 
//array[0] = A
//array[1] = B
//array[2] = C
//array[3] = D
```


### 공백(" ")으로 문자열 잘라서 배열에 넣기

```java
String str = "동해물과 백두산이 마르고 닳도록 하나님이 보우하사 우리나라 만세";
String[] array = str.split(" ");
		    
//출력				
for(int i=0;i<array.length;i++) {
System.out.println(array[i]);
}
		  
//결과값 
//array[0] = 동해물과
//array[1] = 백두산이
//array[2] = 마르고
//array[3] = 닳도록
//array[4] = 하나님이
//array[5] = 보우하사
//array[6] = 우리나라
//array[7] = 만세
```

## 출처

- ([Java] 문자열 자르기(Substring, Split) 사용법 & 예제)[https://coding-factory.tistory.com/126]
