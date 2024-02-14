---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 문자열 붙이기(Append, Concat)"
---
문자열을 이어붙이는 방법에는 3가지가 있다.   
나는 코딩테스트 문제를 풀때 주로 아래와 같이 +를 이용해 붙이는 방법을 사용해왔다. 
```java
String a = "A";
String b = "B";

System.out.println(a + b); // AB 
```

하지만 오늘 포스팅에서는 자바 String클래스의 문자열 붙이기 함수인 Append와 Concat 함수를 활용하는 방법을 적어보도록 하겠다.  
다음에 문제를 풀때 한번 사용해보고 꼭 기억하도록 하자!  

# Append
```java
StringBuilder sb = new StringBuilder("A"); 
sb.append("B");
sb.append("C"); 
System.out.println(sb); // ABC
```
+연산자는 문자열을 StringBuilder로 변환시킨 뒤   
Append로 문자열을 더하고 다시 toString함수로 변환시키는 방식으로 문자열을 붙인다.   


# Concat 
Append는 문자열을 더한 뒤에 toString함수로 변환시키는 방식으로 문자열을 더하지만,    
Concat은 더한 문자열을 String으로 생성해준다.    
```java
String a = "A";
String b = "B"; 

System.out.println(a.concat(b)); //AB
```

두 개의 문자열을 더할 때는 Concat을 사용하는 것이 좋고,    
여러 개를 더해줄 때는 그냥 +연산자를 사용해주는 것이 좋다.   


참고 :    
[https://coding-factory.tistory.com/127?category=758267%20%EC%B6%9C%EC%B2%98:%20https://byul91oh.tistory.com/310%20](https://coding-factory.tistory.com/127?category=758267%20%EC%B6%9C%EC%B2%98:%20https://byul91oh.tistory.com/310%20)