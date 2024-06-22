---
layout: post
categories : CS
tags : Java CS
use_math: true
title:  "[CS] 디자인 패턴 #2"
published : false
---

# 1. 이터레이터 패턴 
이터레이터 패턴은 이터레이터(iterator)를 사용하여 컬렉션(collection)의 요소들에 접근하는 디자인 패턴.    
순회할 수 있는 여러 가지 자료형의 구조와는 상관없이 이터레이터라는 하나의 인터페이스로 순회 가능.    

# 2. 노출모듈 패턴
노출모듈 패턴은 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴. 

# 3. MVC 패턴 
모델(Model), 뷰(View), 컨트롤러(Controller)로 이루어진 디자인 패턴.   
애플리케이션의 구성 요소를 세 가지 역할로 구분하여 개발 프로세스에서 각각의 구성 요소에만 집중해서 개발할 수 있음.   
재사용성, 확장성이 용이하다는 장점.   
애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해지는 단점.   

## 3.1 모델
모델은 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등을 뜻함.   

## 3.2 뷰
inputbox, checkbox, textarea 등 사용자 인터페이스 요소.  
모델을 기반으로 사용자가 볼 수 있는 화면.    
모델이 가지고 있는 정보를 따로 저장하지 않아야 하며 화면에 표시하는 정보만 가지고 있어야 함.    
또한, 변경이 일어나면 컨트롤러에 이를 전달해야 함.  

## 3.3 컨트롤러
하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할을 하며 이벤트 등 메인 로직을 담당함.   
모델과 뷰의 생명주기 관리, 모델이나 뷰의 변경 통지를 받으면 해석하여 각각의 구성 요소에 해당 내용에 대해 알려줌.   

## 3.4 MVC 패턴의 예 스프링 
MVC 패턴을 이용한 대표적인 프레임워크. 자바 플랫폼을 위한 오픈 소스 애플리케이션 프레임워크. 

# 4. MVP 패턴
# 5. MVVM 패턴 







<br>
<br>
<br>
<br>
<br>
<br>

이 포스팅은 책 '면접을 위한 CS 전공지식 노트 (주홍철)'를 참고하였습니다. 