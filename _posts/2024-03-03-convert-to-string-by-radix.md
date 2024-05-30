---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 진법 바꾸기"
published : false
---

# 문자열과 정수를 변환하는 메서드 

|메서드|반환형|내용|
|:---|:---|:---|
|Integer.parseInt(String s)|int|숫자를 표현하는 문자열 s를 정수로 변환|
|Integer.toString(int v)|String|정수 v를 문자열로 변환|
|Long.parseLong(String s)|long|숫자를 표현하는 문자열 s를 정수로 변환|
|Long.toString(long v)|String|정수 v를 문자열로 변환|

위의 메서드들은 모두 10진수를 기준으로 한다.  
여기에서 매개변수 하나만 더 추가하면 진법을 변환할 수 있다.  

# 문자열과 정수를 진법에 따라 변환하는 메서드 

|메서드|반환형|내용|
|:---|:---|:---|
|Integer.parseInt(String s, int radix)|int|radix 진법으로 숫자를 표현하는 문자열 s를 정수로 변환|
|Integer.toString(int v, int radix)|String|정수 v를 radix진법의 문자열로 반환|
|Long.parseLong(String s, int radix)|long|radix 진법으로 숫자를 표현하는 문자열 s를 정수로 변환|
|Long.toString(long v, int radix)|String|정수 v를 radix진법의 문자열로 변환|

예시)   
```java 
String binary = "1010";
int value = Integer.parseInt(binary, 2); // 10
String hex = Integer.toString(value, 16); // a
```

<br>

참고 :    
책) 취업과 이직을 위한 프로그래머스 코딩테스트 문제 풀이 전략 : 자바편 