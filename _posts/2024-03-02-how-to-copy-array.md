---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] Array 객체 복사하는 방법"
published : false
---

Array는 객체이기 때문에 기본 자료형 변수처럼 복사를 하게 되면, 배열의 참조만 복사된다.    
이때, 원본 배열의 값을 변경하면 복사한 배열에서도 값이 변경된다.   

# Object.clone()
```java 
int[] scores = {1, 2, 3, 4, 5}; 
int[] newScores = scores.clone(); 

System.out.println(scores); // [1, 2, 3, 4, 5]
System.out.println(newScores); // [1, 2, 3, 4, 5]

scores[0] = 100; 
newScores[0] = 200; 

// 서로 영향주지 않음! 
System.out.println(scores); // [100, 2, 3, 4, 5]
System.out.println(newScores); // [200, 2, 3, 4, 5]
```

# Arrays.copyOf()
```java 
int[] scores01 = {1, 2, 3, 4, 5, 6, 7}; 

// scores 배열 전체 복사 
int[] scores02 = Arrays.copyOf(scores01, scores.length);

//scores 배열 첫번째 요소부터 3개만 복사 
int[] scores03 = Arrays.copyOf(scores01, 3); 

// scores 배열 특정 범위만 복사 (3번째부터 6번째까지)
int[] scores04 = Arrays.copyOfRange(scores01, 2, 5); 

System.out.println(scores02); // [1, 2, 3, 4, 5, 6, 7]
System.out.println(scores03); // [1, 2, 3]
System.out.println(scores04); // [3, 4, 5, 6]
```

# System.arraycopy()
```java 
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
// Object src : 원본 배열 
// int srcPos : 원본 배열의 복사 시작 지점
// Object dest : 복사할 배열 
// int destPos : 복사할 배열의 복사 시작 지점 
// int length : 복사할 요소의 개수 
```

```java 
int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}; 
int[] copied = new int[15]; 

System.arraycopy(arr, 0, copied, 1, 10); 
System.out.println(Arrays.toString(copied)); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 0, 0, 0, 0]
```
## Arrays.copyOf 와 System.arraycopy() 의 차이 

`Arrays.copyOf` : 새로운 배열 생성 가능, 전부 복사하거나 복사 대상의 객체를 유지시킬 필요가 없을 때 사용 추천.    
`System.arraycopy()`: 복사 길이를 명시해야하거나, 객체를 유지하고자 할 때 사용 추천.     
`Arrays.copyOf` 가 2배 정도 빠름. 

<br>

참고 :     
[https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=gglee0127&logNo=221353415984](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=gglee0127&logNo=221353415984)      
[https://velog.io/@kai6666/Java-System.arraycopy-%EC%99%80Arrays.copyOf%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EB%B0%B0%EC%97%B4-%EB%B3%B5%EC%82%AC](https://velog.io/@kai6666/Java-System.arraycopy-%EC%99%80Arrays.copyOf%EC%9D%98-%EC%B0%A8%EC%9D%B4-%EB%B0%B0%EC%97%B4-%EB%B3%B5%EC%82%AC)