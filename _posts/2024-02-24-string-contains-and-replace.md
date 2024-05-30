---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 문자열 포함 여부 확인 & 문자열 치환"
published : false
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
```java
String replace(CharSequence old, CharSequence new)
```
문자열 중 old 문자열을 new 문자열로 모두 바꾼 문자열을 반환한다.    

```java
String s = "Hello World";

s = s.replace('o', '5'); 
System.out.println(s); // Hell5 W5rld

s = s.replace("Hell5", "Hi");
System.out.println(s); // Hi W5rld
```

## replaceAll()
```java 
String replaceAll(String regex, String replacement)
```
문자열 중 지정된 문자열(regex)과 일치하는 것을 새로운 문자열(replacement)로 모두 변경한다.     

```java
String s = "Hello World"; 

s = s.replaceAll("o", "5"); 
System.out.println(s); // Hell5 W5rld
```

### replace()와 replaceAll()의 차이점 
replace() : 문자열 대치   
replaceAll() : 문자열 대치 + 정규식 적용 가능 

* 정규식이란?  
정규 표현식. 문자열에서 특정한 형태나 규칙을 가진 문자열을 찾아내기 위해 나타내는 패턴.   
. [][^] ^ $ () \n * {} 등의 특수문자와 영문, 숫자 등의 조합으로 구성할 수 있다. 

```java 
//replace()
String s1 = "helloworld에 오신걸 welcome합니다~"; 
s1 = s1.replace("helloworld", "").replace("welcome", ""); 

System.out.println(s1); //에 오신걸 합니다~

//replaceAll()
String s2 = "helloworld에 오신걸 welcome합니다~"; 
s2 = s2.replaceAll("[a-z]", ""); 

System.out.println(s2); //에 오신걸 합니다~
```     

## replaceFirst()
정규 표현식과 일치하는 **첫번째** 하위 문자열을 특정 문자열로 바꾼다. 
```java 
String s = "Hello World"; 

s = s.replaceFirst("o", "5"); 
System.out.println(s); // Hell5 World
```

# 메서드 요약 

## 포함 여부를 검사하는 메서드 

|메서드|반환형|내용|
|:---|:---|:---|
|contains(CharSequence s)|boolean|전달받은 문자열이 원본 문자열에 있는지 검사| 
|startsWith(String prefix)|boolean|원본 문자열이 전달받은 문자열로 시작하는지 검사|
|endsWith(String suffix)|boolean|원본 문자열이 전달받은 문자열로 끝나는지 검사|
|indexOf(String s)|int|전달받은 문자열이 원본 문자열에서 몇번째 인덱스에 있는지 검사|

<br>

## 문자열 치환 메서드 

|메서드|반환형|내용|
|:---|:---|:---|
|replace(char oldChar, char newChar)|String|원본 문자열의 oldChar문자들을 newChar문자로 치환한 문자열을 반환|
|replace(CharSequence target, CharSequence replacement)|String|원본 문자열에서 등장하는 target 문자열을 replacement문자열로 치환해서 반환| 

<br> 


참고 :     
[https://finger-ineedyourhelp.tistory.com/13](https://finger-ineedyourhelp.tistory.com/13)   
[https://bada744.tistory.com/16](https://bada744.tistory.com/16)