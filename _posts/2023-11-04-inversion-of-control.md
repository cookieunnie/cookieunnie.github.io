---
layout: post
categories : Spring
tags : Spring IoC
title:  "[Spring] IoC(Inversion of Control)란?"
---

# IoC(Inversion of Control)란?

제어의 역전.  
메소드나 객체의 호출 작업을 개발자가 결정하는 것이 아니라, 외부에서 결정하는 것을 의미함.

코드를 예시로 들어보자.  
```java
public class A {
    private B b;

    public A(){
        b = new B();
    }
}
```
A 클래스에서 B 필드를 가지고 있고, 생성자 내부에서 직접 생성해서 필드를 초기화하고 있다.  
즉, 객체 생명주기나 메서드의 호출을 개발자가 직접 제어하고 있는 코드이다.  
하지만 이러한 코드 흐름을 spring이 관리한다면 아래와 같을 수 있다.  
```java
public class A {

    @Autowired
    private B b; 
}
```
B 라는 객체가 Spring Container에게 관리되고 있는 Bean이라면 @Autowired를 통해 객체를 주입 받을 수 있다.  
개발자가 직접 B 객체를 관리하지 않고, Spring Container에서 생성, 주입시켜 준 것이다.  
이렇게 코드의 흐름을 개발자가 아닌 외부(Spring)에서 제어하는 것을 '제어의 역전'이라고 한다. 


## 장점
- 객체 간의 결합도(의존성)를 줄이고 응집도를 높여, 유연한 코드를 작성할 수 있음.
- 프로그램의 진행 흐름과 구체적인 구현 분리 가능. 
- 가독성 증가 코드 중복 제거
- 개발자가 비즈니스 로직에 더 집중할 수 있게 함.
- 재사용성 증가

## 단점
- 역제어 구조로 코드를 이해하기 어려울 수 있음.
- 프레임워크에 제어를 맡기는 것으로 너무 의존적이게 될 수 있음.
- 프레임워크가 전반적인 흐름을 관리할 수 있도록 하기 때문에 러닝 커브 존재.
  

# Spring IoC Container
스프링 프레임워크에서 객체를 생성하고 관리하고 책임지고 의존성을 관리해주는 컨테이너  
인스턴스 생성부터 소멸까지의 인스턴스 생명주기 관리를 개발자가 아닌 컨테이너가 대신 수행함.  

## 분류
### DL(Dependency Lookup)
저장소에 저장되있는 Bean에 접근하기 위해 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup하는 것.  

### DI(Dependency Injection)
각 클래스 간의 의존관계를 Bean Definition 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것
- Setter Insection (수정자 주입)
- Contructor Injection (생성자 주입)
- Method Injection (필드 주입)

DL 사용시 컨테이너 종속이 증가하기 때문에 주로 DI 사용


## 종류
스프링 컨테이너가 관리하는 객체 : Bean  
Bean들을 관리 : Bean Factory
- 객체의 생성과 객체 사이의 런타임 관계를 DI 관점에서 볼 때 컨테이너를 BeanFactory라고 함.
- BeanFactory에 여러가지 컨테이너 기능을 추가한 ApplicationContext가 있음.

### BeanFactory
- Bean을 등록, 생성, 조회, 반환 관리 함.  
- BeanFactory계열의 인터페이스만 구현한 클래스는 단순히 컨테이너에서 객체 생성, DI 처리하는 기능만 제공.  
- 팩토리 디자인 패턴을 구현한 것으로 Bean Factory는 빈을 생성하고 분배하는 책임을 지는 클래스.
- Bean을 조회할 수 있는 getBean()메소드가 정의되어 있음.
- 보통은 BeanFactory를 바로 사용하지 않고, 이를 확장한 AppliactionContext 사용.  


### Application Context
* BeanFactory와 같음. 
* 스프링의 각종 부가 기능을 추가 제공.
* BeanFactory와 차별화되는 제공기능
    + 국제화가 지원되는 텍스트 메시지를 관리해줌.
    + 이미지같은 파일 자원을 로드할 수 있는 포괄적인 방법 제공
    + 리스너로 등록된 빈에게 이벤트 발생 알려줌. 
  


  
참고 :  
[https://velog.io/@gillog/Spring-DIDependency-Injection](https://velog.io/@gillog/Spring-DIDependency-Injection)  
[https://oneul-losnue.tistory.com/364](https://oneul-losnue.tistory.com/364)
[https://okeybox.tistory.com/436#IoC%EC%9D%98%20%EC%9E%A5%EC%A0%90%EA%B3%BC%20%EB%8B%A8%EC%A0%90-1](https://okeybox.tistory.com/436#IoC%EC%9D%98%20%EC%9E%A5%EC%A0%90%EA%B3%BC%20%EB%8B%A8%EC%A0%90-1)
[https://dev-coco.tistory.com/80](https://dev-coco.tistory.com/80)