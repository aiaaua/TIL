# AOP

AOP(Aspect Oriented Programming, 관점 지향 프로그래밍)은 어떤 로직을 기준으로 핵심적인 관점과 부가적인 관점을 나누어 보고 그 관점에 따라 공통된 로직이나 그능을 하나로 묶어 각각 모듈화하는 프로그래밍 방법을 의미한다.  

여기서 핵심적인 관점은 핵심 비지니스 로직같은 것이 해당되며, 부가적인 관점은 데이터베이스 연결, 로깅, 파일 입출력 같은 것들이 해당된다.  

![image](https://user-images.githubusercontent.com/55227276/182312576-6ce75eee-0167-4841-b47c-4bc39c3be824.png)

위의 이미지처럼 여러 곳에서 같은 코드가 반복되어 사용되고 있을 경우(= 흩어진 관심사, Crosscutting Concerns)  

흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비지니스 로직에서 분리하여 재사용하는 것이 AOP의 취지이다.  



## AOP 용어와 어노테이션

### AOP 용어

AOP 관련 용어가 많아 순조로운 이해를 위해 별도로 정리하였다.  

#### Aspect

여러 객체에 공통으로 적용되는 관심사항을 `Aspect`라고 하며,  

`Aspect`는 AOP의 중심단위로 `Advice`와 `Pointcut`을 합친 것이다.  

#### Advice

공통 관심 기능을 핵심 로직에 적용하는 시점을 정의하고 있으며,  

`Target` 클래스의 `JoinPoint`로 삽입되어 동작하는 코드이다.  

`Advice`가 `Weaving`되는 인스턴스를 `대상객쳬`라고 한다.  

#### JoinPoint

`Advice`를 적용 가능한 지점을 의미하며,  어플리케이션을 실행할 때 특정 작업이 시작되는 시점이다.  

인스턴스의 생성 시점, 메소드 호출 시점, 예외 발생 시점 등이 모두 `JoinPoint`에 해당한다.  

#### Pointcut

`JoinPoint`의 부분집합으로, 실제로 `Advice`가 적용되는 `JoinPoint`를 나타낸다.  

하나 또는 여러 개의 `JoinPoint`를 하나로 묶은 것을 `Pointcut`이라고 한다.  

하나의 `Pointcut`에는 복수의 `Advice`를 연결할 수 있다.  


#### Weaving

`Advice`를 핵심 로직 코드에 적용하는 것을 말한다. 즉, 공통 코드를 핵심 로직 코드에 삽입하는 것을 의미한다.  

`Aspect` 를 `Target`객체에 제공하여 새로운 프록시 객체를 생성하는 과정이다.  

#### Target

핵심 로직을 구현하는 클래스를 말한다.  

`Target`은 작성한 클래스도 될 수도 있고, 별도의 기능을 추가하고자하는 써드파티 클래스가 될 수도 있다.  

#### Proxy

대상 객체에 `Advice`가 적용된 후 생성된 객체를 의미한다.  



### AOP 어노테이션

조금 전 살펴본 용어 중 `Aspect`와 `Pointcut`에 대한 어노테이션이 있으며

| Annotation  | 의미                                                         |
| ----------- | ------------------------------------------------------------ |
| `@Aspect`   | 자바에서 널리 사용하는 AOP 프레임워크에 포함되며, AOP를 정의하는 Class에 할당 |
| `@Pointcut` | AOP를 적용시킬 지점을 설정(method, annotation 등)            |


Aspect의 실행시점을 지정할 수 있는 어노테이션으로 다음과 같은 것들이 있다.(Advice에 해당하는 부분들)   

| Annotation       | 의미                                                         |
| ---------------- | ------------------------------------------------------------ |
| `@Before`        | Advice의 Target 메소드가 실행되기 이전에 실행                |
| `@After`         | Target 메소드의 결과와 관계 없이 완료되면 실행               |
| `@AfterReturing` | Target 메소드 호출 성공 실행 시(Not Throws)                  |
| `@AfterThrowing` | Target 메소드 호출 실패, 예외 시(Throws)                     |
| `@Around`        | Advice의 Target 메소드를 감싸 Target 메소드 Before/After에 실행 |

추가적으로 자주 같이 사용되는 meta-annotation도 몇 가지 알아보자.  

(여기서 meta-annotation은 built-in 되어있어 다른 annotation에서도 사용되는 annotation들을 말한다.)  

| Annotation       | 의미                                                         |
| ---------------- | ------------------------------------------------------------ |
| `@Target`        | Java compiler가 annotation이 어디에 적용될지 결정하기 위해 사용 |
| `@Ratetion`      | annotation이 실제로 적용되고 유지되는 범위를 의미함             |

> `@Target`은 `ElementType.[XXX]`와 같이 설정하며, PACKAGE, TYPE, ANNOTATION_TYPE, CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PARAMETER 등을 선언할 수 있다.  
> `@Ratetion`는 `RetentionPolicy.RUNTIME`(컴파일 이후에도 참조 가능), `RetentionPolicy.CLASS`(컴파일러가 클래스 참조할 때까지 유효), `RetentionPolicy.SOURCE`(컴파일 전까지만 유효) 세 가지 종류가 있다.  
