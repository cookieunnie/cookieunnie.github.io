---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 문자열 배열 -> 문자열 변환하기"
published : false
---
문자열 배열을 문자열로 변환하는 방법 잊지말기   

# toString()
Arrays클래스의 toString()메서드를 사용하는 방법 
```java
String[] arr = {"Hello", "World"}; 
String s = Arrays.toString(arr); 

System.out.println(s); // Hello World
```

# StringBuilder.Append()
StringBuilder클래스의 Append()메서드를 사용하는 방법 
```java
String[] arr = {"Hello", "World"};
StringBuilder sb = new StringBuilder(); 

for(int i=0; i<arr.length; i++){
    sb.append(arr[i] + " ");
}

String s = sb.toString(); 

System.out.println(s); // Hello World
```

# join()
String 클래스의 join()메서드를 사용하는 방법   
join()메서드의 첫번째 인수는 문자열을 구분하는 기호이며, 두번째 인수는 문자열 배열이다.   
```java
String[] arr = {"Hello", "World"}; 
String s = String.join("_", arr); 

System.out.println(s); // Hello_World
```

# Stream API
java 1.8이상인 경우 Stream API의 Collectors.joining() 메서드를 사용할 수 있음. 
```java
String[] arr = {"Hello", "World"}; 
String s = Arrays
    .stream(arr) // 문자열 배열 전달
    .collect(Collectors.joining()); //문자열 배열을 join

System.out.println(s); // Hello World
```


참고 :    
[https://developer-talk.tistory.com/451#google_vignette](https://developer-talk.tistory.com/451#google_vignette)