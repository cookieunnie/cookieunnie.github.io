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
구현할 때 추상화 레이어를 넣고 이를 기반으로 구현체를 넣어주기 때문에 애플리케이션 의존성 방향이 일관되고, 쉽게 추론할 수 있으며, 모듈 같의 관계들이 더 명확해진다. 

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
# 5. 프록시 패턴과 프록시 서버 