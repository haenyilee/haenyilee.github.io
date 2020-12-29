---
sort: 10
---

# [Java]날짜와 시간 & 형식화

## Calendar 
> 요일, 마지막 날짜

- new 사용해서 클래스 생성하지 않음 
- ```Calendar cal = Calendar.getInstance();```형식으로 클래스 생성
  - 싱글턴 패턴, 즉 메모리를 한 개만 생성하기 때문임
- 값 설정 : ``` cal.set(Calendar.DATE, 1);```
- 값 가져오기 : ```cal.get(Calendar.DATE);```



## Date 
- 시스템 시간
- 날짜, 시간 변환
  -  ```import java.text.*;``` , ```SimpleDateFormat sdf = new SimpleDateFormat("yy-MM-dd");```
  - 년(yyyy yy) 월(M MM) 일(dd d) 시간(h hh H(0~23)) 분(mm) 초(s)
