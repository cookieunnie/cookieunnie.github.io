---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] String과 char타입 변환하기"
---

# String -> char 
## [1] charAt()
```java
String s = "안녕하세요";
char c = s.charAt(0); // "안"
```

## [2] toCharArray() 
```java
String s = "안녕 하세요"
char[] arr = s.toCharArray();

for(char c : arr){
    System.out.println(c);
}

//안
//녕
//    (공백이 출력됨)
//하
//세
//요
```

# char -> String
## [1] valueOf
```java
char c = 'a';
String s = String.valueOf(c); //"a"
```

```java
char[] arr = {'a', 'b', 'c'};
String s = String.valueOf(arr); //"abc"
```

## [2] toString()
```java
char c = 'a';
String s = Character.toString(c); //"a"
```

** toString()사용시에는 배열은 변환 불가.

## [3] "" (추천하지 않는 방법)
```java
char c = 'a';
String s = c + ""; //"a"
```

추천하지 않는 이유  
: valueOf()에 비해 속도가 느린 편이라 추천되지 않음. 

<br><br><br><br><br><br><br><br>


++ 
사실상 알고리즘 풀때마다 자꾸 헷갈려서 한번 써내리며 암기하는 차원에서 쓰는 포스팅이다.    
이 포스팅을 쓴 이후부터는 잊지말자! 


참고    
[https://java119.tistory.com/106](https://java119.tistory.com/106)