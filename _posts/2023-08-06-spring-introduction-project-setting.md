---
layout: post
categories : Spring
tags : Framework Spring Inflean Note
title:  "[강의노트] 스프링 입문 - 프로젝트 환경설정"
---

# 프로젝트 생성
직접 IDE에서 프로젝트를 생성하는 방법도 있지만, [spring initializr](https://start.spring.io/)에서 생성하는 방법을 추천한다. 
spring boot 기반으로 spring 프로젝트를 생성할 수 있게 도와주는 사이트이고, 
실무에서도 많이 사용하는 것 같다. 

![1](/img/in-post/post-spring-intro-project-setting/1.png)

예전에는 maven을 많이 사용했지만, 점차 gradle을 많이 사용하는 추세라고 한다. 
Dependencies에는 미리 필요한 라이브러리들을 검색해서 추가할 수 있다. 
spring web이 꼭 필요하고, Tyhmeleaf는 템플릿 엔진을 이용한 라이브러리인데 MVC 구조로 프로젝트를 실행하기 위해 필요한 것 같다


![2](/img/in-post/post-spring-intro-project-setting/2.png)

GENERATE으로 프로젝트 파일을 내려받고, Intellij에서 프로젝트 구조를 확인할 수 있다. 
요즘은 src 밑에 main, test구조로 생성되는 편이고 (실제 소스 & 테스트 소스)
build.gradle에서 Dependencies에서 추가한 라이브러리들을 확인 할 수 있다. 


![3](/img/in-post/post-spring-intro-project-setting/3.png)

강의에서처럼 src/main/java/hello/hellospring/HelloSpringApplication을 실행하거나, application을 bootrun하면 tomcat server가 8080포트로 실행되었다는 것을 확인할 수 있다. 

![4](/img/in-post/post-spring-intro-project-setting/4.png)

브라우저로 localhost:8080을 확인하면 Error Page가 뜨지만 application이 실행되었음을 확인할 수 있다. 


# 라이브러리 살펴보기


Intellij기준으로 창 우측에 세로로 코끼리 아이콘과 Gradle이라는 표시가 보이지 않는다면 
Command키를 두번 눌러주거나, 화면 좌측 하단 네모 아이콘을 누르면 Dependencies들을 확인할 수 있다. 

![5](/img/in-post/post-spring-intro-project-setting/5.png)

내가 추가한 라이브러리들과, 그 라이브러리들이 필요한 다른 라이브러리들까지 모두 추가되어있는 것을 확인할 수 있다.

이렇게 자동으로 추가되는 것들 중에 tomcat도 있었기 때문에 앞서 프로젝트 생성 시에 tomcat설치와 같은 별도의 과정이 없었음에도 실행되는 것을 확인할 수 있었던 것이다. 


## 스프링 부트 라이브러리
- spring-boot-starter-web
  - spring-boot-starter-tomcat : 웹서버
  - spring-webmvc : 스프링 웹 MVC
- spring-boot-starter-tymeleaf : View
- spring-boot-starter(공통) 
  - spring-boot
    - spring-core
  - spring-boot-starter-logging
    - logback
    - slf4j

+++ 빠르게 잠시 훑고 지나가는 Logging
![6](/img/in-post/post-spring-intro-project-setting/6.png)

요즘에는 slf4j라는 인터페이스를 logback이라는 구현체로 출력을 많이 하는 추세라고 한다. 
성능도 빠르고 여러가지 지원하는 기능들도 많기 때문이다. 
거의 표준화되었고 이제는 start-web에서 자동으로 추가된다. 

## 테스트 라이브러리
- spring-boot-starter-test
  - junit : 테스트 프레임워크
  - mockito : 목 라이브러리 
  - assertj : 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리
  - spring-test : 스프링 통합 테스트 지원
 

# View 환경설정
![7](/img/in-post/post-spring-intro-project-setting/7.png)

Spring boot에서는 static 하위에 위치한 index.html을 먼저 웰컴 페이지로 보여주는 기능을 제공한다. 
[Reference Docs. 참고](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html#web.servlet.spring-mvc.welcome-page)


하지만 위의 방법은 index.html 내용을 단순히 보여주는 정적 방식이다. 
내가 원하는 정보를 동적으로 뷰에 띄워보기 위해 Thymeleaf라는 템플릿 엔진을 사용해볼 수 있다.

![8](/img/in-post/post-spring-intro-project-setting/8.png)
![9](/img/in-post/post-spring-intro-project-setting/9.png)
위와 같이, HelloController와 hello.html을 추가함으로써 /hello 경로에 대한 컨트롤러와 뷰를 생성해준다. 

![10](/img/in-post/post-spring-intro-project-setting/10.png)
다시 localhost:8080에서 /hello경로에 들어가면 내가 컨트롤러에서 모델 안에 넣어주었던 텍스트가 보여지는 것을 확인할 수 있다. 

아래의 순서로 동작한다. 

(1) localhost:8080/hello 진입(요청)

(2) helloController의 GET /hello에 매핑된 메서드 실행 

(3) 해당 메서드에서 'hello'리턴, model안에 'data'라는 attribute에 'hello!'라는 value를 같이 리턴

(4) Controller에서 리턴한 'hello'를 viewResolver가 화면을 찾아준다. (Thymeleaf)

(5) recourse/templates/hello.html로 찾아서 매핑한다. 

(6) 매핑할때 model안의 data를 html에 적용한다. 

(7) model이 적용된 view를 요청받았던 웹 브라우저에 응답해준다.

# 빌드하고 실행하기 

해당 프로젝트 폴더 위치에서 아래 명령어도 프로젝트를 빌드해준다. 

~~~
.gradlew clean build
~~~

![11](/img/in-post/post-spring-intro-project-setting/11.png)

실행 후, /build/libs 위치에 가보면 새로운 .jar파일이 생성되어있는 것을 확인할 수 있다. 

![12](/img/in-post/post-spring-intro-project-setting/12.png)

이렇게 생성된 jar파일을 아래 명령어로 실행시켜주면 localhost:8080에서 이전과 같이 프로젝트가 실행됨을 확인할 수 있다. 

~~~
java -jar {파일명}.jar
~~~

<br>
<br>
<br>
<br>
<br>
*** 이 포스팅은 김영한님의 스프링 입문 강의를 들으며 정리한 내용으로 이루어져 있습니다. ***

[>> 강의 들으러 가기](https://inf.run/g2VX)
