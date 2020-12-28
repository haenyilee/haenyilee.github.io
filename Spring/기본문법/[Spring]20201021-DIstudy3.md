# 20201021-DIstudy(3)

## 참고 
- [초보웹개발자를 위한 스프링5 프로그래밍 입문] 48-58page,75-88page

## 동작 순서
1. Class.forName("Student") : 메모리를 할당한다.
2. setXxx()에 값을 채워준다.
3. init-method 호출
4. **프로그래머 선택** 
5. destroty-method 호출

- 생성과 소멸만 스프링에게 맡기는 방식이다. 


## Student 

### [Student.java]
- 변수 선언
- generate getter/setter


### [app.xml]
- Create a new Spring Bean Definition file
- 태그 선택 : prefix가 이미 만들어져 있는 것이다. 
- context
- p

```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="s1" class="com.sist.di.Student"
      p:name="홍길동"
      p:kor="80"
      p:eng="85"
      p:math="90"
     />
     <bean id="s2" class="com.sist.di.Student"
      p:name="심청이"
      p:kor="80"
      p:eng="85"
      p:math="90"
     />
     <bean id="s3" class="com.sist.di.Student"
      p:name="박문수"
      p:kor="80"
      p:eng="85"
      p:math="90"
     />
</beans>
```

## School

### [School.java]
- school 클래스 데이터형으로 하는 배열 선언
- generate setter

### [app.xml]
- Create a new Spring Bean Definition file
- 태그 선택 : prefix가 이미 만들어져 있는 것이다. 
- context
- p
- 클래스 객체가 들어가면 태그에 ref가 들어간다. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
     <!-- 알고 있어야 한다 : Restful => 한글 처리 -->
     <bean id="school" class="com.sist.di.School">
       <property name="list">
         <list>
           <ref bean="s1"/><!-- list.add(s1) -->
           <ref bean="s2"/>
           <ref bean="s3"/>
         </list>
       </property>
     </bean>
</beans>
```


### [MainClass.java]

```java
package com.sist.di;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainClass {

	public static void main(String[] args) {
        ApplicationContext app=
        		new ClassPathXmlApplicationContext("app.xml");
        School ss=(School)app.getBean("school");
        ss.print();
	}

}
```

