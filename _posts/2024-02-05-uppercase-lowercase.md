---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 대문자와 소문자 변환하기"
---
# 대소문자 변환하기 
## 대문자로 변환
```java
String s = "abc";
s.toUpperCase(); 

System.out.println(s); //ABC
```

## 소문자로 변환 
```java
String s = "ABC"; 
s.toLowerCase();

System.out.println(s); //abc
```

# 대소문자 여부 판단하기 
String이 아닌 char타입임을 주의할 것.
## 대문자인지 여부
```java
public static boolean isUpperCase(char ch)
```
```java
char c = 'A'; 
Character.isUpperCase(c); //true
```

## 소문자인지 여부 
```java
public static boolean isLowerCase(char ch)
```
```java
char c = 'a';
Character.isLowerCase(c); //true 
```