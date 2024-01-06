---
layout: post
categories : Algorithm
tags : Algorithm
use_math: true
title:  "[Algorithm] 순환(Recursion)"
---

# 개념
자기 자신을 호출하는 함수
```java
void func(...){
    ...
    func(...);
    ...
}
```

# 기본 예제 

## 0~n 까지의 합
```java
public static int func(int n){
    if(n==0)
        return 0;
    else 
        return n + func(n-1);
}
```

## $n!$
0! = 1   
n! = n * (n-1)!  (n>0)

```java
public static int factorial(int n){
    if(n==0)
        return 1;
    else
        return n*factorial(n-1);
}
```

## $X^n$
$x^0 = 1$   
$x^n$ = x*$x^{n-1}\qquad(n>0)$

```java
public static double power(double x, int n) {
    if(n==0)
        return 1;
    else
        return x*power(x, n-1);
}
```

## 피보나치 수열 
$f_0 = 0$    
$f_1 = 1$      
$f_n = f_{n-1} + f_{n-2}\qquad (n>1) $      

```java
public int fibonacci(int n) {
    if(n<2)
        return n;
    else
        return fibonacci(n-1) + fibonacci(n-2);
}
```

## 최대공약수 : Euclid Method

```java
public static int gcd(int m, int n){
    if (m<n){
        //m과 n을 바꾼다
        int tmp=m; m=n; n=tmp;
    }
    
    if (m%n==0)
        return n;
    else
        return gcd(n, m%n);
}
```
m $\geq$ n인 두 양의 정수 m과 n에 대해서 m이 n의 배수이면 gcd(m,n) = n이고,    
그렇지 않으면 gcd(m,n) = gcd(n, m%n)이다. 

# 순환 vs 반복
모든 순환함수는 반복문(iteration)으로 변경 가능    
그 역도 성립함. 즉 모든 반복문은 순환(recursion)으로 표현 가능함     
순환함수는 복잡한 알고리즘을 단순하고 알기쉽게 표현하는 것을 가능하게 함    
하지만 함수 호출에 따른 오버헤드가 있음 (매개변수 전달, 액티베이션 프레임 생성 등)

# 순환 알고리즘 설계 
적어도 하나의 base case, 즉 순환되지 않고 종료되는 case가 있어야 함    
모든 case는 결국 base case로 수렴해야함   

```java
if(){
    //base case;
}else{
    //recursion;
}
```

암시적(implicit) 변수를 **명시적(explicit) 매개변수**로 바꾸어라.    
### 순차 탐색 
```java
int search(int [] data, int n, int target){
    for (int i=0; i<n; i++){
        if(data[i]==target)
            return i;
    }
    return -1;
}
```
이 함수의 미션은 data[0]에서 data[n-1] 사이에서 target을 검색하는 것이다.   
하지만 검색 구간의 시작 인덱스 0은 보통 생략한다. 즉 **암시적 매개변수**이다. 

### 매개변수의 명시화 : 순차 탐색
이 함수의 미션은 data[begin]에서 data[end]사이에서 target을 검색한다.   
즉, 검색구간의 시작점을 명시적(explicit)으로 지정한다.    
```java
int search(int [] data, int begin, int end, int target){
    if (begin>end)
        return -1;
    else if (target==data[begin])
        return begin;
    else
        return search(data, begin+1, end, target);
}
```
이 함수를 search(data, 0, n-1, target)으로 호출한다면 바로 전 코드와 동일한 역할을 한다.    

++ 다른 버전
끝 인덱스부터 찾는 방법
```java
int search(int [] data, int begin, int end, int target){
    if(begin>end)
        return -1;
    else if(target==date[end])
        return end;
    else
        return search(data, begin, end-1, target);
}
```

반으로 나눠서 양쪽에서 타겟을 찾는 방법(binary search와 다름)
```java
int search(int [] data, int begin, int end, int target){
    if(begin>end){
        return -1;
    }
    else{
        int middle = (begin+end)/2;
        if (data[middle]==target)
            return middle;
        int index = search(data, begin, middle -1, target);
        if (index != -1)
            return index;
        else
            return search(data, middle+1, end, target);
    }
}
```

### 매개변수의 명시화 : 최대값 찾기
이 함수의 미션은 data[begin]에서 data[end] 사이에서 최대값을 찾아 반환한다. begin<=end라고 가정한다.
```java
int findMax(int [] data, int begin, int end){
    if(begin==end)
        return data[begin];
    else
        return Math.max(data[begin], findMax(data, begin+1, end));
}
```

++ 다른 버전
```java
int findMax(int [] data, int begin, int end){
    if(begin==end)
        return data[begin];
    else
        int middle = (begin+end)/2;
        int max1 = findMax(data, begin, middle);
        int max2 = findMax(data, middle+1, end);
        return Math.max(max1, max2);
}
```

### 이진 탐색 (Binary Search)
데이터들이 크기순으로 정렬되어있을때 적용할 수 있는 검색방법     

items[begin]dㅔ서 items[end] 사이에서 target을 검색한다. 
```java
public static int binarySearch(String[] items, String target, int begin, int end){
    if(begin>end)
        return -1;
    else
        int middle = (begin+end)/2;
        int compResult = target.compareTo(items[middle]);
        if(compResult == 0)
            return middle;
        else if (compResult < 0)
            return binarySearch(items, target, begin, middle-1);
        else
            return binarySearch(items, target, middle+1, end);
}
```
