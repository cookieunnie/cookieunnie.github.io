---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 문자열 포함 여부 확인 & 문자열 치환"
---

# 문자열 포함 여부 확인 
## contains() 
문자열이 특정 문자열을 포함하면 true, 포함하지 않으면 false를 반환한다.    
대소문자를 구분한다.   
```java 
String s = "Hello World";

System.out.println(s.contains("Hello")); // true
System.out.println(s.contains("hello")); // false
```

# 문자열 치환 
## replace()
특정 문자열과 일치하는 하위 문자열을 원하는 문자열로 바꾼다. 
```java
String s = "Hello World";

s = s.replace('o', '5'); 
System.out.println(s); // Hell5 W5rld

s = s.replace("Hell5", "Hi");
System.out.println(s); // Hi W5rld
```

## replaceAll()
정규 표현식과 일치하는 하위 문자열을 특정 문자열로 바꾼다. 
```java
String s = "Hello World"; 

s = s.replaceAll("o", "5"); 
System.out.println(s); // Hell5 W5rld
```

## replaceFirst()
정규 표현식과 일치하는 **첫번째** 하위 문자열을 특정 문자열로 바꾼다. 
```java 
String s = "Hello World"; 

s = s.replaceFirst("o", "5"); 
System.out.println(s); // Hell5 World
```

참고 : 
[https://finger-ineedyourhelp.tistory.com/13](https://finger-ineedyourhelp.tistory.com/13)