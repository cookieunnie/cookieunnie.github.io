---
layout: post
categories : Spring
tags : Framework Spring Inflean Note
title:  "[강의노트] 스프링 입문 - 스프링 빈과 의존관계"
---

# 스프링 빈과 의존관계 
@Controller, @Service, @Repository와 같은 어노테이션들을 이용해 스프링 빈을 등록할 수 있다. 

@Autowired는 각 빈의 연관관계를 나타내준다.

스프링 빈을 등록하는 방법은 2가지가 있다.

## 컴포넌트 스캔과 자동 의존관계 설정
(1) @Component 어노테이션이 있으면 스프링 빈으로 자동 등록된다. @Controller, @Service, @Repository들은 @Component를 포함하기 때문에 스프링 빈으로 자동 등록되는 것이다. 

(2) 내가 실행하려는 application이 위치한 패키지의 하위에 생성된 컴포넌트들만 스프링이 자동으로 스캔한다. 

(3) 생성자에 @Autowired를 사용하면 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입하기 때문에, 생성자가 1개만 있으면 @Autowired는 생략할 수 있다. 

(4) 스프링은 스프링 컨테이너에 스프링 빈을 등록할 때, 싱글톤으로 등록하기때문에 (하나만 등록해서 공유) 같은 스프링 빈이면 모두 같은 인스턴스이다. 싱글톤이 아니게 설정할 수 있지만 기본적으로는 싱글톤으로 생성한다. 


## 자바 코드로 직접 스프링 빈 등록하기
@Configuration이 붙은 클래스에서 @Bean 빈 등록과 연관관계 설정이 가능하다. 

~~~~
@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService(){
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
        return new MemoryMemberRepository();
    }
}
~~~~

의존성 주입에는 필드 주입, Setter 주입, 생성자 주입 3가지가 있다. 

생성자 주입이 가장 추천되는 방법이며, Setter 주입은 public이기 때문에 변경될 가능성이 있으므로 추천되지 않은 방법임. 

주의 : @Autowired를 통한 의존성 주입은 스프링이 관리하는 객체에서만 동작한다. 스프링 빈으로 등록하지 않고 직접 생성한 객체에서는 동작하지 않는다. 


<br>
<br>
<br>
<br>
<br>
*** 이 포스팅은 김영한님의 스프링 입문 강의를 들으며 정리한 내용으로 이루어져 있습니다. ***

[>> 강의 들으러 가기](https://inf.run/g2VX)
