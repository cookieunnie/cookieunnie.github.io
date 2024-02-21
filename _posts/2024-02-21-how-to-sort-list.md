---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 리스트 정렬하기"
---

# Collections.sort()
```java 
public static void sort(List<T> list)
public static void sort(List<T> list, Comparator<? super T> c)
```

## 오름차순
Collections.sort()에 리스트를 인자로 전달하여 **오름차순** 정렬한다.   
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

public class SortArrayList{
    public static void main(String[] args){
        ArrayList<String> list = new ArrayList<>(Arrays.asList("C", "A", "B", "a"));

        Collections.sort(list); 

        System.out.println(list); // A, B, C, a
    }
}
```

## 내림차순 
Collections.sort()에 리스트와 `Collections.reverseOrder()를 전달하여 **내림차순** 정렬한다.   
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

public class SortArrayList{
    public static void main(String[] args){
        ArrayList<String> list = new ArrayList<>(Arrays.asList("C", "A", "B", "a"));

        Collections.sort(list, Collections.reverseOrder()); 
        
        System.out.println(list); // a, C, B, A
    }
}
```

## 대소문자 구분없이 정렬하기 
Colletions.sort()에 `String.CASE_INSENSITIVE_ORDER`를 전달하면 **대소문자 구분없이**,  **오름차순**으로 정렬한다.   
아래 예시에서 "a"와  "A"는 같은 순위로 취급되므로, 원래의 순서를 유지한다.   

Collections.sort()에 `Collections.reverseOrder()`에 `String.CASE_INSENSITIVE_ORDER`를 추가해서 전달하면 **대소문자 구분없이**, **내림차순**으로 정렬한다.    
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

public class SortArrayList{
    public static void main(String[] args){
        ArrayList<String> list = new ArrayList<>(Arrays.asList("C", "A", "B", "a"));

        // 대소문자 구분없이 오름차순
        Collections.sort(list, String.CASE_INSENSITIVE_ORDER); 
        System.out.println(list); // a, A, B, C

        // 대소문자 구분없이 내림차순 
        Collections.sort(list, Collections.reverseOrder(String.CASE_INSENSITIVE_ORDER)); 
        System.out.println(list); // C, B, a, A
    }
}
```

# List.sort()
Java 8 이후 가능해진 방법이다.    
```java
default void sort(Comparator<? super E> c)
```

## 오름차순
```java 
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;

public class SortArrayList{
    public static void main(String[] args){
        ArrayList<String> list = new ArrayList<>(Arrays.asList("C", "A", "B", "a"));
        
        list.sort(Comparator.naturalOrder());
        
        System.out.println(list); // A, B, C, a
    }
}
```

## 내림차순
```java 
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;

public class SortArrayList{
    public static void main(String[] args){
        ArrayList<String> list = new ArrayList<>(Arrays.asList("C", "A", "B", "a"));
        
        list.sort(Comparator.reverseOrder()); 

        System.out.println(list); // a, C, B, A
    }
}
```

## 대소문자 구분없이 정렬
```java 
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;

public class SortArrayList{
    public static void main(String[] args){
        ArrayList<String> list = new ArrayList<>(Arrays.asList("C", "A", "B", "a"));

        list.sort(String.CASE_INSENSITIVE_ORDER); 
        System.out.println(list); // a, A, B, C

        list.sort(Collections.reverseOrder(String.CASE_INSENSITIVE_ORDER));
        System.out.println(list); // C, B, A, a
    }
}
```

# 사용자 정의 
사용자가 정의한 객체를, 사용자가 원하는 방식으로 정렬하기 위해서 Comparable 인터페이스를 구현한다.   
객체의 정렬 방식을 지정할 수도 있고, Comparator 인터페이스를 구현할 수도 있다. 

## Comparable
Collections.sort() 메소드는 객체를 정렬할 때, 해당 객체의 Comparable을 구현한 compareTo() 메소드를 참조하여 정렬 순서를 결정한다.    
따라서, 정렬할 객체가 Comparable interface를 구현하고, compareTo() 메소드 안에 정렬 기준이 정의된다면, Collections.sort()메소드를 사용하여 객체를 정렬할 수 있다. 

```java
import java.util.ArrayList; 
import java.util.Collections;

public class SortArrayList(){
    public static void main(String[] args){
        ArrayList<Fruit> list = new ArrayList<>(); 
        list.add(new Fruit("Apple", 2000)); 
        list.add(new Fruit("Orange", 3000)); 
        list.add(new Fruit("Banana", 1000)); 

        // 가격순 오름차순 정렬 
        Collections.sort(list);
        System.out.println(list); // [Banana, 1000], [Apple: 2000], [Orange: 3000]

        // 가격순 내림차순 정렬 
        Collections.sort(list, Collections.reverseOrder());
        System.out.println(list); // [Orange: 3000], [Apple: 2000], [Banana, 1000]
    }
}

class Fruit implements Comparable<Fruit>{
    private String name; 
    private int price; 

    public Fruit(String name, int price){
        this.name = name;
        this.price = price; 
    }

    @Override
    public int compareTo(Fruit fruit){
        if(fruit.price < price){
            return 1; 
        }else if(fruit.price > price){
            return -1; 
        }
        return 0; 
    }

    @Override
    public String toString(){
        return "[" + this.name + ": " + this.price + "]";
    }
}
```

## Comparator
사용자가 직접 Comparator interface를 implements 하여 Comparator를 만들 수 있다.    
이 Comparator는 Collections.sort() 또는 List.sort() 메소드의 파라미터로 전달되어 정렬 기준이 된다. 

```java
import java.util.ArrayList; 
import java.util.Collections;
import java.util.Comparator; 

public class SortArrayList{
    public static void main(String[] args){
        ArrayList<Fruit> list = new ArrayList<>(); 
        list.add(new Fruit("Apple", 2000)); 
        list.add(new Fruit("Orange", 3000)); 
        list.add(new Fruit("Banana", 1000)); 

        // 가격순 오름차순 정렬
        Collections.sort(list, new FruitPriceComparator()); 
        System.out.println(list); // [Banana: 1000], [Apple: 2000], [Orange: 3000]

        // 가격순 내림차순 정렬
        Collections.sort(list, new FruitPriceComparator().reversed()); 
        System.out.println(list); // [Orange: 3000], [Apple: 2000], [Banana: 1000]

        // 이름순 오름차순 정렬
        Collections.sort(list, new FruitNameComparator()); 
        System.out.println(list); // [Apple: 2000], [Banana: 1000], [Orange: 3000]

        // 이름순 내림차순 정렬
        Collections.sort(list, new FruitNameComparator().reversed());
        System.out.println(list); // [Orange: 3000], [Banana: 1000], [Apple: 2000]

    }
}

class FruitPriceComparator implements Comparator<Fruit>{
    @Override 
    public int compare(Fruit f1, Fruit f2){
        if(f1.price> f2.price){
            return 1; 
        }else if(f1.price < f2.price){
            return -1; 
        }

        return 0; 
    }
}

class FruitNameComparator implements Comparator<Fruit>{
    @Override 
    public int compare(Fruit f1, Fruit f2){
        return f1.name.compareTo(f2.name); 
    }
}

class Fruit{
    String name; 
    int price; 

    public Fruit(String name, int price){
        this.name = name; 
        this.price = price; 
    }

    @Override
    public String toString(){
        return "[" + this.name + ": " + this.price +"]";
    }
}
```



참고 :     
[https://hianna.tistory.com/569](https://hianna.tistory.com/569)