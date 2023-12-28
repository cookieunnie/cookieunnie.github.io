---
layout: post
categories : Spring
tags : Spring
title:  "[Spring] DI(Dependency Injection)란?"
---

# DI(Dependency Injection)란?
객체를 직접 생성하는 것이 아니라 외부에서 생성 후 객체 간의 관계(의존성)를 결정해주는 방식   

지난 IoC 포스팅에서 언급되었듯이, Spring에서 의존 관계를 주입(DI)하는 방법은 3가지이다.   
- Setter Insection (수정자 주입)
- Contructor Injection (생성자 주입)
- Method Injection (필드 주입)


## Setter Insection (Setter 주입)
Spring에서 @Autowired 어노테이션을 사용해 Setter 메서드를 통해 주입하는 방법   
Spring 3.x 버전까지는 DI 권장 방식이였지만, 4.3 이후부터는 생성자 주입방법이 권장된다.  
NPE(Null Poinrt Exception) 발생할 수 있다.    
Setter 주입, 필드 주입은 모두 생성자 이후에 호출되므로, 필드에 final 키워드를 사용할 수 없다.  

```java
public class Injection {

    private InjectionService injectionService;

    @Autowired
    public void setInjectionService(InjectionService injectionService){
        this.injectionService = injectionService;
    }
}
```


## Contructor Injection (생성자 주입)
현재 가장 권장되는 의존 관계 주입 방식이다.    
    - 하나의 생성자가 존재할 때 필드 주입의 대부분의 단점을 극복한다.    
    - Spring 4.3부터는 @Autowired가 생략 가능해서 최근에는 생성자 딱 1개 두고, @Autowired를 생략하는 방법을 주로 사용한다.   
    - Lombok 라이브러리의 @RequiredArgsConstructor를 함께 사용하면 생성자를 생략 가능해서 코드가 깔끔해진다.

오직 생성자 주입만이 final 키워드를 사용할 수 있고, 생성자를 통해 주입되는 방식이다.    
    - final 키워드를 사용하기에 생성자로 인해 인스턴스가 생성될 때 1번만 할당된다. 

final 키워드를 사용해서 값이 한번 할당되면 변경할 수 없기에 객체의 불변성(Immutability)가 보장된다.    
초기에 할당되기때문에 NPE(Null Pointer Exception)이 절대 발생하지 않는다. 

```java
public class Injection {

    private InjectionService injectionService;

    //@Autowired -> Spring 4.3 부터는 생략 가능
    public Injection(InjectionService injectionService){
        this.injectionService = injectionService;
    }
}
```

생성자 주입이 가장 좋은 방법이지만, 매번 생성자를 만들어야하기도 하고, 코드가 길어질 수 있다.    
여기서 롬복(Lombok) 라이브러리를 사용하면 생성자 또한 어노테이션으로 생략 가능하다.   

```java
@RequiredArgsConstructor
public class Injection {

    private InjectionService injectionService;
}
```
@RequiredArgsConstructor 어노테이션은 롬복(Lombok)의 어노테이션 중 하나이다.   
final 키워드가 붙은 주입에만 생성자를 만들어주며, final 키워드를 사용하지 않으면 @AllArgsContructor를 사용하면 된다.


## Method Injection (필드 주입)
Spring에서 @Autowired 어노테이션을 사용해 객체 내 필드에 선언해서 주입하는 방법.    

Bean으로 등록된 객체를 사용하고자 하는 클래스에 Field로 선언한 뒤 @Autowired를 붙여주면 자동으로 의존 관계가 주입된다.    

간편하게 의존 관계 주입이 가능하지만 참조 관계를 눈으로 확인하기 어렵고, 순환 참조를 막을 수 없다.     
필드 주입, Setter 주입은 모두 생성자 이후에 호출되므로, 필드에 final 키워드를 사용할 수 없다.

```java
public class Injection {
    @Autowired
    private InjectionService injectionService;
}
```
코드가 간결해진다는 장점이 있지만,   
SOLID 원칙 중 단일 책임 원칙(SRP)을 위반하고,    
Unit Test가 어려우며,    
final 키워드를 사용할 수 없어 불변성이 보장되지 않는다는 단점이 있다.


### 덧붙이자면,

참고한 블로그에 아주 정리가 잘 되어있는 것 같다.   
덕분에 읽으면서 IoC와 늘 헷갈리던 DI의 개념에 대해 좀 더 정확히 알게 되었고,   
왜 실무에서 순환 참조 문제가 발생하였었는지 알게 됬다.    
또, Setter 주입을 해둔 코드를 다시 수정하고싶은 마음이 굴뚝 같아졌다.    
잊지말자...! 생성자 주입 >>>>> Setter 주입 > 필드 주입 


  
참고 :  
[https://backendcode.tistory.com/249#head6](https://backendcode.tistory.com/249#head6)