---
sort: 1
---

# JSON 기본 문법


## JSON 이란?

- JavaScript Object Notation의 줄임말
- 브라우저-서버 간 통신에서 데이터를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷이다.
- 본래는 자바스크립트에서 파생되었지만 편의성 덕분에 모든 언어/플랫폼에서 사용하게 되었다.

## JSON의 구조
- JSON은 자료형 리스트와 딕셔너리를 섞어놓은 것처럼 생겼다. 

### 1. []
- JSON Array(배열)

```JSON
[
  {
      "id":1,
      "name": "Haenyi",
      "age":29, 
  },
  {
      "id":2,
      "name": "Sunny",
      "age":34, 
  }
]
```

### 2. {}
- JSON Object(클래스)

```JSON
{
    "id":1,
    "name": "Haenyi",
    "age":29, 
}
```

## JSON의 데이터형
### 1. String
- 문자열
- 따옴표(`""`)로 둘러 쌓여 있음

### 2. long
- 정수형
- 따옴표(`""`)로 둘러 쌓여 있지 않음

## JSON Array 에 이름 부여하기 

- JSON Object가 열리는 `{`앞에 `""`를 추가하여 만들 수 있다.

```json
{
   "employee": 
   [ {
    " id " : 1,
    "name" : "Jay",
    "age" : 27
  },
  {
     " id " : 2,
     "name" : "MJ",
     "age" : 25 
  }
  ]
}
```
