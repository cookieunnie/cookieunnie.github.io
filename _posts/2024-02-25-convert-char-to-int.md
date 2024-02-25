---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] Char를 Int로 변환하기"
---
String -> Int를 더 자주 변환했었다보니 가끔 Char -> Int 변환 때 알쏭달쏭할때가 있었으므로   
오늘은 Char를 Int로 변환하는 방법에 대해 정리해보고자 한다.       

# Ascii code 이용     

|문자|10진수|문자|10진수|문자|10진수
|:--:|:---:|:-:|:-:|:-:|:-:|
|0|48|A|65|a|97|
|1|49|B|66|b|98|
|2|50|C|67|c|99|
|3|51|D|68|d|100|
|:|:|:|:|:|:|
|9|57|Y|89|y|121|
|||Z|90|z|122|
         
         

```java 
char c1 = '9';
int i1 = c1 - '0'; //57 - 48 = 9
```
<br>

# Character.getNumericValue() 이용 
```java 
char c1 = '9'; 
Character.getNumericValue(c1) // 9
```
<br>

참고 :     
[https://frhyme.github.io/java/java_basic02_char_to_int/](https://frhyme.github.io/java/java_basic02_char_to_int/)      
[https://blog.naver.com/PostView.nhn?blogId=jysaa5&logNo=221831226674](https://blog.naver.com/PostView.nhn?blogId=jysaa5&logNo=221831226674)