# IoC와 DI

Spring에서 일반적인 Java 객체를 개발자가 new로 생성하여 관리하지 않고 Spring Container에 맡기는데  

이렇게 객체 관리의 제어 권한이 개발자에서 프레임워크로 넘어가는 것을 IoC(Inversion of Control, 제어의 역전)라고 한다.  

또, 구성요소 간의 의존관계가 소스코드 내부가 아닌 외부 설정파일을 통해 정의되도록 하는 것을 DI(Dependency Injection, 의존성 주입)라고 한다.  



DI를 하는 3가지 방법과 IoC를 위한 bean 등록 5가지 방법에 대해 알아보고, 예제를 통해 일반적인 자바코드에 DI와 IoC를 적용하는 예제를 살펴보자.  



## DI의 3가지 방법

Spring에서 의존성을 주입하는 방법은 다음과 같이 세 가지로 나뉜다.  

- Field Injection
- Setter Injection
- Constructor Injection (생성자 주입) -> 스프링에서 권장하는 방식



### Field Injection

`Field Injection`은 의존성을 주입하고 싶은 필드에`@Autowired` 어노테이션을 붙여 의존성을 주입하는 방법이다.  

다음과 같이 동작한다.  

1. 주입받으려는 빈의 **생성자를 호출**하여 빈을 찾거나 빈 팩토리에 등록  
2. 생성자 인자에 사용하는 **빈을 찾거나 만듦**  
3. 주입하려는 빈 객체의 수정자를 호출하여 **주입**   

이 방식의 문제점은 **런타임에서 의존성을 주입**하기 때문에 **의존성을 주입하지 않아도 객체가 생성될 수 있다**는 점이다.  



### Setter Injection

`Setter Injection`은 setter 메서드에 `@Autowired` 어노테이션을 붙여 의존성을 주입하는 방법이다.  

다음과 같이 동작한다.  

1. 주입받으려는 빈의 **생성자를 호출**하여 빈을 찾거나 빈 팩토리에 등록  
2. 생성자 인자에 사용하는 **빈을 찾거나 만듦**   
3. 필드에 **주입**    

이 역시 `Field Injection`과 동일한 문제점이 있다.  

따라서 Spring은 다음에 소개되는 `Constructor Injection`을 권장한다.  



### Constructor Injection

`Constructor Injection`은 생성자를 사용하여 의존성을 주입하는 방법이다.  

이는 앞의 둘과 어떻게 다르게 동작하는지 살펴보자.  

1. **생성자의 인자에 사용되는 빈을 찾거나 팩토리에서 만든다.**  
2. 찾은 인자 빈으로 주입하려는 **생성자를 호출**한다.  

이 방식은 객체가 생성되는 시점에 빈을 주입하므로 의존성이 주입되지 않아 발생할 수 있는 `NullPointerException`을 방지할 수 있다.  

또한 앞서 두 방식에서는 불가한 final 선언이 가능하며 순환참조를 방지할 수 있다.  



## DI 어노테이션

의존성 주입 어노테이션은 다음과 같은 것들이 있다.    

| 어노테이션 | 설명                                                         |
| ---------- | ------------------------------------------------------------ |
| @Autowired | 변수 위에 설정하여 해당 타입의 객체를 찾아서 자동으로 할당   |
| @Qualifier | 특정 객체의 이름을 이용하여 의존성을 주입할 때 사용          |
| @Injection | Java에서 제공하는 어노테이션으로 @Autowired와 동일함         |
| @Resource  | Java에서 제공하는 어노테이션으로 @Autowired와 @Qualifier의 기능을 결합한 것 |

이 중에서 Spring에서 제공하는 `@Autowired`와 `@Qualifier`에 대해 사용법을 알아보자.  



### @Autowired

`@Autowired`는 가장 흔히 쓰이는 방법이다.  

생성자, 메서드, 멤버변수 위에 모두 사용이 가능하며(대부분 멤버변수 위에 선언) 자동으로 할당해준다.  

변수의 타입을 체크하고 객체가 메모리에 존재하는지 확인한 후 객체를 변수에 주입한다.  

만약 `@Autowired`가 붙은 객체가 메모리에 없다면 `NoSuchBeanDefinitionException`을 발생시킨다.  

```java
public class TestAutowired {
	
    @Autowired
    private TestDao testDao;
    
    public TestAutowired() {
    	// TestAutowired 객체생성
    }
}
```



### @Qualifier

`@Qualifier`는 자동이 아닌 사용할 의존 객체를 선택해야할 때 쓴다.  

의존성 주입의 대상이 되는 특정 객체를 찾고, 조건을 만족하는 객체에 의존성을 주입한다.  

단, 의존성 주입 대상이 되는 타입의 객체가 두 개 이상일 때는 컨테이너가 어떤 객체를 할당해야할지 판단할 수 없기 때문에 오류가 발생한다.  

이럴 경우에는 `@Autowired`아래에 `@Qualifier`를 사용한다.  

```java
public class TestQualifier {

    @Autowired
    @Qualifier("testDao")
    private SampleDao sampleDao;
    
    public TestQualifier() {
    	// TestQualifier 객체 생성
    }
}
```



## IoC를 위한 bean등록 5가지 방법

bean 설정을 위한 방법은 다음과 같은 것들이 있다.

| 방법                           | 설명                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| xml 설정 파일 작성             | xml 설정 파일에 `bookService`와 `bookRepository`를 빈으로 등록하고 `bookService`의 특성으로 `bookRepository`를 등록하여 참조하게 함 |
| xml 설정 파일 작성(Spring 2.5) | auto-scan 기능이 추가되어 빈을 등록하기 위해 `@Component`어노테이션을 클래스 위에 붙여줌, xml 설정 파일에 빈을 직접 등록하는 것보다 쉬움 |
| Java Config                    | java 설정 파일을 만들어 빈을 등록하고 `ApplicationContext`의 `AnnotationConfigApplicationContext`를 통해 빈 팩토리를 받아옴 |
| Java Config + ComponentScan    | java config에 빈을 직접 등록하지 않고 `@ComponentScan` 어노테이션을 메인 클래스에 등록하여 그 아래에 연관된 클래스에 등록된 모든 빈들을 알아서 스캔 및 등록 |
| Spring Boot                    | `@SpringBootApplication` 어노테이션을 사용, `@ComponentScan`과 `@Configuration`이 내포되어 설정 파일 자체가 하나로 완성되어있음, 빈 등록과 의존성 주입을 알아서 해줌 |

이 중에서 앞으로 사용하게될 Spring Boot를 이용한 방법에 대해 자세히 알아보자.  

### @SpringBootApplication

사실 `@SpringBootApplication`을 사용하는 방법은 매우 간단하다.   

이 어노테이션이 있는 위치부터 설정을 읽어가므로 이 어노테이션이 붙는 클래스는 프로젝트의 최상단에 위치해야한다.  

즉, 프로젝트 최상단의 클래스에 `@SpringBootApplication` 어노테이션을 붙이면 된다.  

```java
package com.example.myapplication;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```

`@SpringBootApplication`은 이전에 이미 간단히 살펴본 바와 같이 `@SpringBootConfiguration`, `@EnableAutoConfiguration`, `@ComponentScan` 세 가지가 합쳐진 것인데,  각각의 역할은 다음과 같다.  

- `@SpringBootConfiguration`: Spring에 빈 팩토리를 위한 오브젝트의 설정을 담당하면서도 auto-scan이 가능함
- `@EnableAutoConfiguration`: spring.factories에 미리 정의된 빈들을 등록
- `@ComponentScan`: `@Component`와 같은 어노테이션이 붙어있는 클래스들을 찾아 빈으로 등록

