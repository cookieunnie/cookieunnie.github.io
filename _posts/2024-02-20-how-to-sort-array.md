---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 배열 정렬하기"
---
# Arrays.sort() 

배열에 저장된 객체의 타입과 상관없이, Comparable이 구현된 객체라면 모두 정렬할 수 있다.  
(Comparable이 구현되지 않는 클래스라면, 구현하고 정렬 가능하도록 만들 수 있음)
기본적으로 객체는 Comparable()이 구현되어있다.    
sort()는 Comparable에 의해 리턴되는 값을 비교하여 배열을 정렬한다.    

## int 배열 정렬 
### 오름차순 정렬
Arrays.sort()로 int 배열을 정렬하면 **오름차순**으로 정렬된다.    
```java
int[] arr = {3, 4, 5, 1, 2}; 

Arrays.sort(arr); 

System.out.println(Arrays.toString(arr)); // 1, 2, 3, 4, 5
```

### 내림차순 정렬
Arryas.sort()에 `Collections.reverseOrder()`를 전달하면 **내림차순**으로 정렬된다.    
`Collections.reverseOrder()`는 Comparator객체이며, 역순으로 정렬해준다.   
Comparator는 직접 구현할 수 있지만, 미리 정의된 Collections 함수 사용할 수 있음.    
```java 
Integer[] arr = {1, 2, 5, 4, 3}; 

Arrays.sort(arr, Collections.reverseOrder()); 

System.out.println(Arrays.toString(arr));  // 5, 4, 3, 2, 1
```

### 내림차순 Comparator 직접 구현 
byte, char, double, short, long, int, float같은 PrimitiveType의 배열에는 적용이 불가능하니 Integer같은 Wrapper Class를 이용해야하는 것 주의   
```java
Integer[] arr = {1, 2, 5, 4, 3}; 

Arrays.sort(arr, new Comparator<Integer>() {
    @Override
    public int compare(Integer i1, Integer i2){
        return i2 - i1; 
    }
}); 

System.out.println(Arrays.toString(arr)); // 5, 4, 3, 2, 1
```

위 코드를 Lambda를 이용하여 더 짧게 구현할 수 있다.   
```java 
Integer[] arr = {1, 2, 5, 4, 3}; 

Arrays.sort(arr, (i1, i2) -> i2 - i1); 

System.out.println(Arrays.toString(arr)); // 5, 4, 3, 2, 1
```

### 부분 정렬 
Arrays.sort()에 정렬 범위의 처음 index와 마지막 index를 전달하면 일부분만 정렬 가능하다.    
```java
int[] arr = {1, 2, 3, 5, 4}; 

Arrays.sort(arr, 3, 4); 

System.out.println(Arrays.toString(arr)); // 1, 2, 3, 4, 5
```

## String 정렬
### 오름차순 정렬
int 정렬과 동일하다. 아스키 코드를 기준으로 정렬한다. 

### 내림차순 정렬
int 정렬과 동일하다. 

### 문자열 길이순 정렬
문자열 길이로 정렬을 하고 싶을 때는 직접 Comparator를 구현해야한다.   
기본적으로 구현된 Comparator는 아스키 코드로 정렬하기 때문인다.    
```java 
String[] arr = {"Apple", "Kiwi", "Orange", "Banana", "Watermelon"}; 

Arrays.sort(arr, new Comparator<String>(){
    @Override
    public int compare(String s1, String s2){
        return s1.length() - s2.length(); 
    }
});

System.out.println(Arrays.toString(arr)); 
// Kiwi, Apple, Orange, Banana, Watermelon
```

이 역시 Lambda를 사용하면 더 간결하게 구현 가능하다.   
```java
String[] arr = {"Apple", "Kiwi", "Orange", "Banana", "Watermelon"}; 

Arrays.sort(arr, (s1, s2) -> s1.length() - s2.length()); 

System.out.println(Arrays.toString(arr)); 
// Kiwi, Apple, Orange, Banana, Watermelon
```

## 객체 배열 정렬
Custom 클래스의 객체를 갖고 있는 배열도 sort()로 정렬할 수 있다.   
객체를 비교할 수 있도록 Custom 클래스에 Comparable을 구현하면 가능하다.   
Comparable 인터페이스의 compareTo() 메서드를 오버라이드하거나   
익명 인터페이스 java.util.Comparator를 구현한 Class내 compare() 메서드를 
원하는 정렬 조건으로 오버라이드하면 된다.      
```java 
public static class Fruit implements Comparable<Fruit>{
    private String name; 
    private int price; 
    public Fruit(String name, int price){
        this.name = name; 
        this.price = price; 
    }

    @Override
    public String toString(){
        return "{name: " + name + ", price: " + price + "}";
    }

    @Override
    public ine compareTo(@NotNull Fruit fruit){
        return this.price - fruit.price;
    }
}

Fruit[] arr = {
    new Fruit("Apple", 100), 
    new Fruit("Kiwi", 500), 
    new Fruit("Orange", 200), 
    new Fruit("Banana", 50), 
    new Fruit("Watermelon", 880)
}; 

Arrays.sort(arr); 

System.out.println(Arrays.toString(arr)); 
// {name: Banana, price: 50}, {name: Apple, price: 100}, 
// {name: Orange, price: 200}, {name: Kiwi, price: 500}, 
// {name: Watermelon, price: 880}
```

# Stream API
java 8 이상을 사용한다면 배열이나 컬렉션을 다루는 Stream API를 이용해 정렬할 수 있다.   
Stream을 이용하면 Lambda로 표현하여 좀 더 간결하게 표현할 수 있다.    
```java 
String[] arr = {"A", "C", "D", "F", "E", "B"}l 

String streamSortASC = Stream.of(arr).sorted().collect(Collectors.joining()); 
Strimg streamSortDESC = Stream.of(arr).sorted(Comparator.reverseOrder()).collect(Collectors.joining()); 

String streamSortASC_Lambda = Stream.of(arr).sorted((s1, s2) -> s1.compareTo(s2)).collect(Collectors.joining());
Strimg streamSortDESC_Lamda = Stream.of(arr).sorted((s1, s2) -> s2.compareTo(s1)).collect(Collectors.joining()); 

System.out.println(streamSortASC); //ABCDEF
System.out.println(streamSortDESC); //FEDCBA
System.out.println(streamSortASC_Lambda); //ABCDEF
System.out.println(streamSortDESC_Lamda); //FEDCBA
```

참고 :     
[https://codechacha.com/ko/java-sorting-array/](https://codechacha.com/ko/java-sorting-array/)   
[https://ifuwanna.tistory.com/232](https://ifuwanna.tistory.com/232)