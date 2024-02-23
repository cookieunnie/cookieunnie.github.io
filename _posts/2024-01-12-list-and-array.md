---
layout: post
categories : Java
tags : Java
use_math: true
title:  "[Java] 배열과 리스트 변환하기"
---

# 배열 -> List 
## [1] Arrays.asList()
```java
import java.util.Arrays;
import java.util.List;

public class ArrayConversion{
    public static void main(String[] args){
        String[] arr = {"A", "B", "C"};
        List<String> list = Arrays.asList(arr);
    }
}
```

## [2] new ArrayList<>(Arrays.asList())
Arrays.asList()는 고정 길이의 원배열의 List View를 리턴한다.    
그래서 Arrays.asList()로 배열을 List로 변환한 뒤 값을 추가하는 것이 불가능하다.  
원래의 배열 값을 변경하면 List의 값도 같이 변경된다.   
변경 뒤의 List값을 변경하면 원래의 배열 값도 같이 변경된다.    

```java
import java.util.Arrays;
import java.util.List;
import java.util.ArrayList;

public class ArrayConversion{
    public static void main(String[] args){
        String[] arr = {"A", "B", "c"};
        List<String> list1 = Arrays.asList(arr);
        List<String> list2 = new ArrayList<String>(Arrays.asList(arr));

        // 배열 값 변경
        arr[0] = "D";

        // 리스트 값 변경
        list1.set(2, "E");

        // list에 값 추가 
        list2.add("D");

        // 배열과 List가 값을 변경했을 때 서로 영향을 받는 것을 알 수 있음. 
        System.out.println(list1.get(0)); //D
        System.out.println(list1.get(1)); //B
        System.out.println(list1.get(2)); //E
        System.out.println(Arrays.toString(arr)); //["D", "B", "E"];


        // 배열값을 변경하여도 List 값이 변경되지 않음. 
        // List의 길이 변경 가능.
        System.out.println(list2.get(0)); //A
        System.out.println(list2.get(1)); //B
        System.out.println(list2.get(2)); //C
        System.out.println(list2.get(3)); //D
        
    }
}
```

### int 배열을 List로 변환할 경우
Arrays.atList()로 배열을 List로 변환할 때,   
배열의 원소가 int와 같은 primitive type인 경우 좀 다른 결과를 리턴한다.   
```java
import java.util.Arrays;
import java.util.List;

public class IntArrayConvertToList{
    public static void main(String[] args){
        int[] arr = {1, 2, 3};
        List<int[]> intList1 = Arrays.asList(arr);
        List<Integer> intList2 = new ArrayList<>();
    
        // 올바르게 변환하는 방법
        // [1] 반복문 사용
        for(int element : arr){
            intList2.add(element);
        }

        // [2] Stream 사용
        List<Integer> intList3 
            = Arrays.stream(arr)
                .boxed()  
                .collect(Collectors.toList());

        //primitive 타입을 Wrapper클래스로 형변환해주기 않았기 때문 (int -> Integer) 
        System.out.println(intList1.size()); //1
        System.out.println(intList1.get(0)); //객체 주소 출력
        System.out.println(Arrays.toString(intList1.get(0))); //[1,2,3]

        //올바르게 변환된 List 
        System.out.println(intList2.size()); //3
        System.out.println(intList2); //[1,2,3]
        System.out.println(intList3.size()); //3
        System.out.println(intList3); //[1,2,3]


    }
}
```
Stream을 사용하여 변환한 방법에서 boxed()는 primitive stream 값들을 wrapper 클래스로 바꿔준다.   
이와 같은 방법으로 double, long 등 다른 primitive 타입의 배열들도 변환 가능하다.   

## [3] Collectors.toList()
Java 8 이후부터, Stream을 사용하여 변환 가능하다.

```java
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class ArrayConversion{
    public static void main(String[] args){
        String[] arr = {"A", "B", "C"};

        List<String> list = Stream.of(arr).collect(Collectors.toList());
    }
}
```

# List -> 배열
## [1] toArray()
java.util.List의 toArray()는 파라미터로 전달받은 객체의 길이가 원본 List보다 작을 경우,  
자동으로 원본 List의 size 크기로 배열을 만들어준다.   
원본 List의 길이보다 배열의 크기를 더 크게 지정하면,   
배열의 나머지 인덱스는 null로 채워진다. 

```java
import java.util.ArrayList;
import java.util.Arrays;

public class ArrayListConversion{
    public static void main(String[] args){
        //ArrayList
        ArrayList<String> arrList = new ArrayList<String>();
        arrList.add("A");
        arrList.add("B");
        arrList.add("C");

        //ArrayList를 배열로 변환
        int arrListSize = arrList.size();
        String arr[] = arrList.toArray(new String[arrListSize]);
    }
}
```

## Integer ArrayList를 int 배열로 변환할 경우
int와 같은 primitive 타입은 toArray()를 사용할 수 없다.  
따라서 아래와 같은 방법으로 변환할 수 있다.    
```java
public static voic main(String args[]){
    List<Integer> list = new ArrayList<>(); 
    list.add(1); 
    list.add(2); 
    list.add(3);

    // 방법 1
    int[] arr1 = new int[list.size()];
    for(int i=0; i<list.size(); i++){
        arr[i] = list.get(i).intValue();
    }

    // 방법 2
    // 자바가 자동으로 각 요소의 Integer -> int형으로 unboxing해준다. 
    int[] arr2 = list.stream()
                .mapToInt(i -> i)
                .toArray(); 

    // 방법 3
    //intValue 메서드를 통해 Integer -> int형으로 변경해준다. 
    int[] arr3 = list.stream()
                .mapToInt(Integer::intValue)
                .toArray();

    // 방법 4
    int[] arr4 = list.stream()
                .filter(i -> i != null)
                .mapToInt(i -> i)
                .toArray();
}
```
<br><br><br>


++ 
사실상 알고리즘 풀때마다 자꾸 헷갈려서 한번 써내리며 암기하는 차원에서 쓰는 포스팅이다.    
이 포스팅을 쓴 이후부터는 잊지말자! 


참고    
[https://hianna.tistory.com/551](https://hianna.tistory.com/551)
