---
layout: post
categories : CS
tags : Java CS
use_math: true
title:  "[CS] 디자인 패턴 #1"
published : false
---

디자인 패턴이란?
프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약'형태로 만들어 놓은 것. 

# 1. 싱글톤 패턴
하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴     
하나의 클래스를 기반으로 단 하나의 인스턴스를 만들어 이를 기반으로 로직을 만드는 데 쓰임.   
보통 데이터베이스 연결 모듈에 사용함.    

## 1.1 장점
1. 한 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어든다. 

## 1.2 단점
1. 한 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 의존성이 높아진다. 
2. TDD(Test Driven Development) 할 때 걸림돌이 될 수 있다.     
단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다. 하지만 싱글톤 패턴은 미리 생성된 인스턴스 하나를 기반으로 구현하는 패턴이르모 각 테스트마다 독립적인 인스턴스를 만들기가 어려움. 

## 1.3 의존성 주입 
모듈간의 결합을 강하게 만들 수 있다는 싱글톤 패턴의 단점을 의존성 주입(DI, Dependency Injection)을 통해 결합을 좀 느슨하게 만들어 해결할 수 있음.    

의존성 = 종속성    
A가 B에 의존성이 있다 = B의 변경 사항에 대해 A도 변해야 한다.   

메인 모듈이 직접 다른 하위 모듈들에 대한 의존성을 주기 보다는   
-> 중간에 의존성 주입자가 이 부분을 가로채 간접적으로 의존성을 주입하는 방식    

이를 통해 상위 모듈은 하위 모듈에 대한 의존성이 떨어지게 된다.(디커플링이 된다)

좀 더 자세히 알고 싶다면, 아래의 글을 참고하자. 
[https://cookieunnie.github.io/spring/2023/12/28/dependency-injection/](https://cookieunnie.github.io/spring/2023/12/28/dependency-injection/)

### 1.3.1 의존성 주입의 장점 
모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅과 마이그레이션이 쉬워진다.   
구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어주기 때문에 애플리케이션 의존성 방향이 일관되고, 쉽게 추론할 수 있으며, 모듈 간의 관계들이 더 명확해진다. 

### 1.3.2 의존성 주입의 단점
모듈들이 더욱더 분리되므로 클래스 수가 늘어나 복잡성이 증가할 수 있고 약간의 런타임 페널티가 생기기도 한다. 

### 1.3.3 의존성 주입의 원칙
"상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다. 또한, 둘 다 추상화에 의존해야 하며, 이때 추상화는 세부 사항에 의존하지 말아야 한다."    

# 2. 팩토리 패턴
팩토리 패턴은 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴.    
상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴.     

(1) 느슨한 결합 : 상위 클래스와 하위 클래스가 분리되기 때문     
(2) 유연성 : 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문.    
(3) 유지보수성 : 객체 생성 로직이 따로 떼어져 있기 때문에 코드를 리팩토링하더라도 한 곳만 고칠 수 있게 됨.   

ex) 라떼, 아메리카노, 우유 레시피같은 구체적인 내용이 들어있는 하위 클래스가 컨베이어 벨트를 통해 전달되고 상위 클래스인 바리스타 공장에서 이 레시피들을 토대로 우유를 생상하는 생산 공정을 생각해볼 것. 

```java
abstract class Coffee{
    public abstract int getPrice(); 

    @Override
    public String toString(){
        return "Hi this coffee is " + this.getPrice();
    }
}

class CoffeeFactory{
    public static Coffee getCoffee(String type, int price){
        if("Latte".equalsIgnoreCase(type)) return new Latte(price);
        else if("Americano".equalsIgnoreCase(type)) return new Americano(price);
        else{
            return new DefaultCoffee();
        }
    }
}

class DefaultCoffee extends Coffee{
    private int price;

    public DefaultCoffee(){
        this.price = -1;
    }

    @Override
    public int gerPrice(){
        return this.price;
    }
}

class Latte extends Coffee{
    private int price; 

    public Latte(int price){
        this.price = price;
    }

    @Override
    public int getPrice(){
        return this.price;
    }
}

class Americano extends Coffee{
    private int price; 

    public Americano(int price){
        this.price = price;
    }

    @Override
    public int getPrice(){
        return this.price;
    }
}

public class Main{
    public static void main(String[] args) {
        Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
        Coffee ame = CoffeeFactory.getCoffee("Americano", 3000);
        System.out.println("Factory latte :: " + latte);
        System.out.println("Factory ame :: " + ame);
    }
}

/*
Factory latte :: Hi this coffee is 4000
Factory ame :: Hi this coffee is 3000
*/
```

# 3. 전략 패턴
정책 패턴이라고도 함.  
객체의 행위를 바꾸고 싶은 경우 직접 수정하지 않고 전략이라고 부르는 '캡슐화한 알고리즘'을 컨텍스트 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴.   

ex) 어떤 것을 살 때 네이버페이, 카카오페이 등 다양하게 결제하듯이 결제 방식의 '전략'만 바꿔서 두가지 방식으로 결제하는 것을 구현   

```java
interface PaymentStrategy{
    public void pay(int amount);
}

class KAKAOCareStrategy implements PaymentStrategy{
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;

    public KAKAOCareStrategy(String nm, String ccNum, String cvv, String expiryDate){
        this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.dateOfExpiry = expiryDate;
    }

    @Override
    public void pay(int amount){
        System.out.println(amount + " paid using KAKAOCard.");
    }
}

class NAVERCareStrategy implements PaymentStrategy{
    private String emailId;
    private String password;

    public NAVERCareStrategy(String email, String pwd){
        this.emailId = email;
        this.password = pwd;
    }

    @Override
    public void pay(int amount){
        System.out.println(amount + " paid using NAVERCard.");
    }
}

class Item{
    private String name;
    private int price;
    
    public Item(String name, int cost){
        this.name = name;
        this.price = cost;
    }

    public String getName(){
        return name;
    }

    public int getPrice(){
        return price;
    }
}

class ShoppingCart{
    List<Item> items;

    public ShoppingCart(){
        this.items = new ArrayList<Item>();
    }

    public void addItem(Item item){
        this.items.add(item);
    }

    public void removeItem(Item item){
        this.items.remove(item);
    }

    public int calculateTotal(){
        int sum = 0; 
        for(Item item : items){
            sum += item.getPrice();
        }
        return sum;
    }

    public void pay(PaymentStrategy paymentMethod){
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}

public class Main{
    public static void main(String[] args){
        ShoppingCart cart = new ShoppingCart();

        Item A = new Item("A", 100);
        Item B = new Item("B", 300);

        cart.addItem(A);
        cart.addItem(B);

        // pay by NEVERCard
        cart.pay(new NAVERCareStrategy("example@naver.com", "1234"));

        // pay by KAKAOCard
        cart.pay(new KAKAOCareStrategy("Hong Gil Dong", "123456789", "123", "12/01"));

    }
}

/*
400 paid using NAVERCard.
400 paid using KAKAOCard.
*/
```

# 4. 옵저버 패턴
주체가 어떤 객체의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴.    

주체 : 객체의 상태 변화를 보고 있는 관찰자     
옵저버 : 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 '추가 변화 사항'이 생기는 객체들을 의미함.    

주체와 객체를 따로 두지 않고 상태가 변경되는 객체를 기반으로 구축하기도 함. 
옵저버 패턴을 활용한 서비스로는 트위터가 있음.    
=> 내가 어떤 사람인 주체를 '팔로우'했다면 주체가 포스팅을 올리게 되면 알림이 '팔로워'에게 감.     

옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC 패턴에도 사용됨.    
주체라고 볼 수 있는 모델에서 변경 사항이 생겨 update() 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러 등이 작동함.  

```java
import java.util.ArrayList;
import java.util.List;

interface Subject{
    public void register(Observer obj);
    public void unregister(Observer obj);
    public void notifyObservers();
    public Object getUpdate(Observer obj);
}

interface Observer{
    public void update();
}

class Topic implements Subject{
    private List<Observer> observers;
    private String message;

    public Topic(){
        this.observers = new ArrayList<>();
        this.message = "";
    }   

    @Override
    public void register(Observer obj){
        if(!observers.contains(obj)) observers.add(obj);
    }

    @Override
    public void unregister(Observer obj){
        observers.remove(obj);
    }

    @Override
    public void notifyObservers(){
        this.observers.forEach(Observer::update);
    }

    @Override
    public Object getUpdate(Observer obj){
        return this.message;
    }

    public void postMessage(String msg){
        System.out.println("Message sended to Topic: " + msg);
        this.message = msg;
        nofifyObservers();
    }
}

class TopicSubscriber implements Observer{
    private String name;
    private Subject topic;

    public TopicSubscriber(String name, Subject topic){
        this.name = name;
        this.topic = topic;
    }

    @Override 
    public void update(){
        String msg = (String) topic.getUpdate(this);
        System.out.println(name + ":: got message >> " + msg);
    }
}

public class Main{
    public static void main(String[] args){
        Topic topic = new Topic();
        Observer a = new TopicSubscriber("a", topic);
        Observer b = new TopicSubscriber("b", topic);
        Observer c = new TopicSubscriber("c", topic);
        topic.register(a);
        topic.register(b);
        topic.register(c);

        topic.postMessage("amumu is op champion!!");
    }
}

/*
Message sended to Topic : amumu is op champion!!
a:: got message >> amumu is op champion!!
b:: got message >> amumu is op champion!!
c:: got message >> amumu is op champion!!
*/
```

topic을 기반으로 옵저버 패턴을 구현한 예제.    
topic : 주체이자 객체.    

## 자바 : 상속과 구현 
예제 코드에 나온 자바의 상속과 구현의 특징과 차이.    

**상속** 
상속은 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용하며 자식 클래스에서 추가 및 확장을 할 수 있는 것.     
재사용성, 중복성의 최소화.   

**구현**
구현은 부모 인터페이스를 자식 클래스에서 재정의하여 구현하는 것.    
상속과 달리 반드시 부모 클래스의 메서드를 재정의하여 구현해야함.    

**상속과 구현의 차이**
상속은 일반 클래스, abstract 클래스를 기반으로 구현하며,    
구현은 인터페이스를 기반으로 구현함.    


# 5. 프록시 패턴과 프록시 서버 
## 5.1 프록시 패턴
프록시 패턴은 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로채 대상 객체 앞단의 인터페이스 역할을 하는 디자인 패턴.    
이를 통해 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용함.    
프록시 객체로 쓰이기도 하지만 프록시 서버로도 활용됨.    

## 5.2 프록시 서버 
프록시 서버는 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 컴퓨터 시스템이나 응용 프로그램.   

### 5.2.1 프록시 서버로 쓰는 nginx
nginx는 비동기 이벤트 기반의 구조와 다수의 연결을 효과적으로 처리 가능한 웹 서버이며, 주로 Node.js 서버 앞단의 프록시 서버로 활용됨.    
이를 통해 익명 사용자가 직접적으로 서버에 접근하는 것을 차단하고, 간접적으로 한 단계를 더 거치게 만들어서 보안을 강화할 수 있음.   

### 5.2.2 프록시 서버로 쓰는 CloudFlare
CloudFlare는 전 세계적으로 분산된 서버가 있고 이를 통해 어떠한 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스.   
웹 서버 앞단에 프록시 서버로 두어 DDOS 공격 방어나 HTTPS 구축에 쓰임.    
서비스 배포 이후 의심스러운 트래픽이 많이 발생하면 이때문에 많은 클라우드 서비스 비용이 발생할 수도 있는데, 이때 CloudFlare가 의심스러운 트래픽인지 먼저 판단해 CAPTCHA 등을 기반으로 이를 일정 부분 막아주는 역할도 수행함.    

- CDN(Content Delivery Network)   
각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크를 말함. 이를 통해 사용자가 웹 서버로부터 콘텐츠를 다운로드하는 시간을 줄일 수 있음. 

- DDOS 공격 방어    
DDOS는 짧은 기간 동안 네크워크에 많은 요청을 보내 네트워크를 마비시켜 웹 사이트의 가용성을 방해하는 사이버 공격 유형.    
CloudFlare는 의심스러운 트래픽, 특히 사용자가 접속하는 것이 아닌 시스템을 통해 오는 트래픽을 자동으로 차단해서 DDOS 공격으로부터 보호함. CloudFlare의 거대한 네트워크 용량과 캐싱 전략으로 소규모 DDOS 공격은 쉽게 막아낼 수 있으며 이러한 공격에 대한 방화벽 대시보드도 제공함. 

- HTTPS 구축   
서버에서 HTTPS를 구축할 때 인증서를 기반으로 구축할 수도 있음. 하지만 CloudFlare를 사용하면 별도의 인증서 설치 없이 좀 더 손쉽게 HTTPS를 구축할 수 있음.  

### 5.2.3 CORS와 프론트엔드의 프록시 서버
CORS(Cross-Origin Resource Sharing)는 서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘.   
프론트엔드 개발시 프론트엔더 서버를 만들어서 백엔드 서버와 통신할 때 주로 CORS 에러를 마주치는데, 이를 해결하기 위해 프론트엔드에서 프록시 서버를 만들기도 함.   

- 오리진   
프로토콜과 호스트 이름, 포트의 조합.   
예를 들어, https://kundol.com:12010/test 라는 주소에서 오리진은 https://kundol.com:12010을 뜻한다. 

예를 들어 프론트엔드에서는 127.0.0.1:3000으로 테스팅을 하는데 백엔드 서버는 127.0.0.1:12010이라면 포트 번호가 다르기 때문에 CORS 에러가 남. 이때 프록시 서버를 둬서 프론트엔드 서버에서 요청되는 오리진을 127.0.0.1:12010으로 바꾸는 것. 
CORS 에러도 해결되고, 다양한 API 서버와의 통신도 매끄럽게 할 수 있음.   






<br>
<br>
<br>
<br>
<br>
<br>

이 포스팅은 책 '면접을 위한 CS 전공지식 노트 (주홍철)'를 참고하였습니다. 