---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 2진수와 10진수 변환하기"
published : false
---
코딩테스트 문제를 풀다가 String타입으로 받은 이진수의 합을 구하는 부분에서 알게된 내용을 정리하고자 한다.   
거의 메모에 가까운 포스팅이 될 것 같지만, toBinaryString을 분명 Binary뭐더라 할 것이 분명하므로 한 번 적어보고자 한다.   
문제 링크 > [https://school.programmers.co.kr/learn/courses/30/lessons/120885](https://school.programmers.co.kr/learn/courses/30/lessons/120885)

# 10진수 -> 2진수 
Integer클래스의 toBinaryString 사용 
```java
Integer.toBinaryString(int i); 
```

# 2진수 -> 10진수 
Integer클래스의 parseInt 사용
```java
Integer.parseInt(String s, int n진수);

//예시 
Integer.parseInt("101", 2); // 2진수 형태의 "101"을 10진수로 변환 
```



# 제출 코드 
```java
class Solution {
    public String solution(String bin1, String bin2) {
        //2진수 -> 10진수
        int num1 = Integer.parseInt(bin1, 2);
        int num2 = Integer.parseInt(bin2, 2);
        
        //10진수 -> 2진수
        return Integer.toBinaryString(num1 + num2);
    }
}
```

