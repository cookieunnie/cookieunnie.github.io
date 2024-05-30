---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 문자열 자르기 (substring)"
published : false
---
코딩테스트를 낮은 난이도부터 차근히 풀면서 아마 가장 많이 사용했던 메서드가 substring이 아니였나싶다.    
이 또한 시간이 지나면 특히 인덱스까지 포함이 되어 잘렸는지 아니였는지 헷갈릴 것이 분명하므로 남겨두려 한다.      


# substring(int index) 
해당 인덱스부터 마지막 인덱스까지 잘라서 반환한다. 


```java
String s = "Hello World"; 

System.out.println(s.substring(6)); // World
```

|H|e|l|l|o| |<span style="color:red">**W**</span>|<span style="color:red">**o**</span>|<span style="color:red">**r**</span>|<span style="color:red">**l**</span>|<span style="color:red">**d**</span>|   
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|0|1|2|3|4|5|<span style="color:red">**6**</span>|<span style="color:red">**7**<span>|<span style="color:red">**8**</span>|<span style="color:red">**9**</span>|<span style="color:red">**10**</span>|


# substring(int startIndex, int endIndex)
startIndex에 해당하는 인덱스부터, endIndex 바로 전 인덱스까지 잘라서 반환한다. 
```java
String s = "Hello World"; 

System.out.println(s.substring(2,7)); // llo W
```

|H|e|<span style="color:red">**l**</span>|<span style="color:red">**l**</span>|<span style="color:red">**o**</span>| |<span style="color:red">**W**</span>|o|r|l|d|   
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|0|1|<span style="color:red">**2**</span>|<span style="color:red">**3**</span>|<span style="color:red">**4**</span>|<span style="color:red">**5**</span>|<span style="color:red">**6**</span>|7|8|9|10|



<br>
substring안에 인자가 2개 있을때는 마지막 인덱스는 포함하지 않는다는 것 주의!    
인자 갯수와 상관없이 첫 인덱스에 해당하는 문자도 포함한다는 것 주의!   