# Spring Boot란 무엇일까?

지난 번에 우리는 Spring에 대해 알아보는 시간을 가졌었다.   

이제 본격적으로 Spring을 사용해 웹 개발을 시작해보려고하는데, 그를 위해 Spring Boot를 사용한다고 한다.   

그렇다면 Spring Boot란 무엇일까?  



## Spring Boot의 정의

Spring Boot는 기존의 Spring 프레임워크에 톰캣 서버를 내장하고 여러 편의 기능들을 추가한 `웹 프레임워크`이다.    

Spring에 이어서 프레임워크라는 말이 다시 등장했는데 잠깐 다시 짚고 넘어가보자면, 프레임워크 자체는 `라이브러리 + 설계도`를 제공하는 뼈대를 말한다고 했었다.  

웹 프레임워크를 해석해보자면, 웹 개발에 자주 사용되는 것들의 뼈대를 만들어둔 것을 말한다.  

가령 쿠키 처리, 세션 처리, 로그인/로그아웃 처리 등 다양한 것들을 미리 만들어두고 개발자가 직접 설정하지 않더라도 자주 사용되는 기본 설정이 다 알아서 되어있는 것이다.(물론 변경도 가능하다.)   

따라서 웹 프레임워크로 웹을 개발한다면 모든 기능을 직접 다 개발할 필요가 없으며, 이미 웹 프레임워크에 있는 기능을 익혀 사용하기만 하면 된다.   



## Spring Boot 왜 쓸까?

정의에서 웹 프레임워크라는 것만 살펴보아도 알 수 있듯이, 생산성이 매우 뛰어나며 따라서 비용도 적게 든다.   

Spring을 사용하기 위한 필수 설정 파일을 모두 작성하는 것은 사실상 불가능에 가깝기 때문에, 기존 설정 파일의 내용을 그대로 카피하거나 검색을 통해 설정해주어야했다.   

Spring Boot를 사용하면 이런 일들을 안해도 된다니 얼마나 멋진 일일까!



## Spring Boot의 장점

Spring Boot가 단순히 편리함만을 가지고 있는 것은 아니다.  

여러 가지 장점이 또 있는데 이에 대해 살펴보자.  

### 1. 라이브러리 관리 자동화

Spring Boot에는 `starter 모듈`이 있는데, starter는 특정 목적을 달성하기 위한 의존성 그룹을 뜻한다.  

`spring-boot-starter-*`와 같은 형식으로 지정할 수 있으며, `*`에는 애플리케이션의 이름을 작성해준다.  

이 starter에 애플리케이션 별 라이브러리를 등록하면 의존성을 간단히 관리할 수 있다.  

> _사실 이 부분은 그렇게 크게 와닿지 않아서 태영님 설명 필요_

자주 사용되는 starter는 다음과 같다. 

| starter name                   | description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| spring-boot-starter            | 스프링 부트의 코어, auto-configuration, logging, yaml 제공   |
| spring-boot-starter-aop        | AOP(관점 지향 프로그래밍)을 위한 스타터                      |
| spring-boot-starter-batch      | 스프링 배치를 사용하는데 필요한 스타터                       |
| spring-boot-starter-data-jpa   | 스프링 데이터 JPA와 하이버네이트를 사용하는데 필요한 스타터  |
| spring-boot-starter-data-redis | 메모리 저장 방식의 저장소인 레디스와 레디스 설정 자동화 스타터 |
| spring-boot-starter-data-rest  | 스프링 데이터 저장소 방식에 맞춘 REST API를 제공하는데 사용하는 스타터 |
| spring-boot-starter-thymeleaf  | 타임리프 템플릿 엔진을 사용하는데 필요한 스타터              |
| spring-boot-starter-jdbc       | 톰캣 JDBC 커넥션 풀에 사용하는 스타터                        |
| spring-boot-starter-security   | 각종 보안에 사용하는 스프링 시큐리티 스타터                  |
| spring-boot-starter-oauth2     | Oauth2 인증에 사용하는 스타터                                |
| spring-boot-starter-validation | 자바 빈 검증에 사용하는 스타터                               |
| spring-boot-starter-web        | 웹을 만드는데 사용하는 스타터                                |

### 2. 라이브러리 버전 자동 관리

기존 Spring 라이브러리는 버전을 직접 입력해야 했지만,   

Spring Boot는 pom.xml에 스프링 부트 버전을 입력해두면 Spring 라이브러리 뿐만 아니라 third party 라이브러리들까지도 호환되는 버전으로 알아서 다운 및 관리를 해준다.  

> _사실 이 부분도 그렇게 크게 와닿지 않아서 태영님 설명 필요_
>
> _앞의 장점과 다른게 뭔가요_?

### 3. 설정 자동화

Spring Boot는 `@EnableAutoConfiguration` 어노테이션을 선언해 Spring에서 자주 사용했던 설정들을 알아서 등록해준다.  

Spring Boot의 main 클래스 상위에는 `@SpringBootApplication`이라는 어노테이션이 있는데, 

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

이는 의 가장 기본적인 설정을 선언해주는 것이며,  

해당 어노테이션을 들어가보면 다음과 같이 다시 어노테이션들이 선언되어있다.  

크게 `@SpringBootConfiguration`,  `@ComponentScan`, `@EnableAutoConfiguration`이 합쳐진 것이다.  

```java
@Target(value = {ElementType.TYPE})
@Retention(value = RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
    @ComponentScan.Filter(type = FilterType.CUSTOM, classes = {TypeExcludeFilter.class}),
    @ComponentScan.Filter(type = FilterType.CUSTOM, classes = {AutoConfigurationExcludeFilter.class})})
public @interface SpringBootApplication {
    ...
}
```

이 어노테이션들 중 핵심은  `@ComponentScan`, `@EnableAutoConfiguration` 이렇게 두 가지 이다.  

이 어노테이션들은 bean으로 등록해주는 역할을 하는데, 여기서 bean이란 Spring 컨테이너가 관리하는 자바 객체를 말한다.

(조금 더 상세히 알아보자면, 이전에 알아본 Spring의 특징 중 IoC(제어의 역전)에서 우리는 객체의 생성과 사용자의 제어권을 Spring에 넘겼는데 이렇게 Spring에 의해 관리되는 자바 객체를 bean이라고 한다.)  

먼저 `@ComponentScan`은 `@Component`,  `@Configuration`, `@Service`, `@Repository`, `@Controller` 등의 어노테이션을 스캔하여 bean으로 등록해주는 어노테이션이다.  

그리고 우리가 이야기하는 설정자동화와 관련된 것이 바로 `@EnableAutoConfiguration`인데,  

`@EnableAutoConfiguration`은 사전 정의 파일에 나열된 라이브러리들을 bean으로 등록해주는 어노테이션이다.  

사전 정의 파일은 `spring.factories`이며 이 파일에 정의된 라이브러리 들 중 특정 조건이 만족되는 경우만 bean으로 등록하게 된다.  

따라서 이 어노테이션에 의해 우리가 Spring에서 자주 사용했던 설정들을 아무런 세팅 없이 사용하는 듯한 느낌을 받게 된다.  

~~(Spring Boot의 마법이라고 불린다고 한다.)~~

### 4. WAS 필요 없음

Spring Boot의 정의에서 잠깐 등장했던 말인데, Spring Boot는 톰캣 서버를 내장하고 있다고 했었다.  

톰캣 서버가 무엇일까? 일단 톰캣은 흔히 WAS의 한 종류라고 말한다.  

그래서 Spring Boot가 톰캣을 내장했기 때문에 별도의 WAS가 필요없다는 것이 장점인 것이다.  

그렇다면 WAS란 무엇일까?

WAS(Web Application Server)는 `Web server + Web container`의 개념이다.  

![image](https://user-images.githubusercontent.com/55227276/180273146-b180739b-d2bd-4ea0-9355-1818709976e0.png)

Web server는 HTTP 프로토콜을 통해 읽힐 수 있는 HTML같은 정적 컨텐츠를 처리하는 것인데,  

Web server에서 JSP를 요청하면 Web container는 JSP파일을 서블릿으로 변환하여 컴파일을 수행하고 그 결과를 HTML로 구성하여 정적인 data로 만들어 Web server에 전달한다.  

방금 말을 이해하기 위해 JSP와 서블릿에 대한 개념을 간단히 알아보자면 JSP(JavaServer Pages)란 HTML 코드에 JAVA코드를 넣어 동적 웹페이지를 생성하는 도구를 말하는 것이며 서블릿(Servlet)은 웹페이지를 동적으로 생성하기 위한 서버측 프로그램을 말한다.   

즉, WAS는 동적 컨텐츠를 처리하는데 최적화되어있다.  

정적 컨텐츠를 요청했을 경우 단순히 Web server에서 정적 data를 처리하며

동적 컨텐츠를 요청했을 경우 Web server와 Web container를 모두 이용하는 WAS가 이를 처리하는 것이다.  

Spring Boot대신 Spring만 사용하여 웹 애플리케이션을 개발한다면 이를 실행시킬 수 있는 WAS가 필요하게 되는데 WAS는 종류도 다양하고 방식도 제각각이어서 WAS를 세팅하는 것도 매우 복잡한 일이다.  

그러나 Spring Boot에는 이미 WAS의 한 종류인 톰캣이 내장되어있고 자동으로 적용되기 때문에 바로 웹 애플리케이션을 실행할 수 있게 된다.  

심지어 배포되는 jar 파일에도 톰캣이 내장되어있어 패키징하여서도 웹 애플리케이션을 실행시킬 수 있다.  

### 5. 보안

웹 프로그램을 만들 때 어려운 분야 중 하나가 보안인데, Spring Boot는 이런 공격들을 기본으로 막아주고 있다.  

예를 들어 SQL 인젝션, XSS(cross-site scripting), CSRF(cross-site request forgery), 클릭재킹(clickjacking)과 같은 보안 공격을 기본으로 막아주고 있기 때문에 Spring Boot를 사용한다면 이런 보안 공격에 대한 코드를 직접 짤 필요가 없다.

>SQL 인젝션은 악의적인 SQL을 주입하여 공격하는 방법이다.
>XSS는 자바스크립트를 삽입해 공격하는 방법이다.
>CSRF는 위조된 요청을 보내는 공격 방법이다.
>클릭재킹은 사용자의 의도하지 않은 클릭을 유도하는 공격 방법이다.



## Reference

- https://wikidocs.net/160047
- https://cheershennah.tistory.com/194
- https://velog.io/@woo00oo/SpringBoot-%EC%8A%A4%ED%83%80%ED%84%B0

- https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.starters
- https://bamdule.tistory.com/31

- https://velog.io/@falling_star3/Spring-Boot-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88bean%EA%B3%BC-%EC%9D%98%EC%A1%B4%EA%B4%80%EA%B3%84
- https://helloworld-88.tistory.com/71
- https://javacpro.tistory.com/43
