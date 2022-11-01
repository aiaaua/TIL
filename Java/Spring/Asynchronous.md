# Asynchronous

외부 API에 작업을 요청하였는데 외부 API 서버에서 요청을 처리하는데 오랜 시간이 걸리는 경우 비동기 방식으로 처리하면 효율적인 작업 처리가 가능하다.  

Spring에서 비동기처리를 하는 방법은 매우 간단한데, 다음과 같이 2단계로 이루어진다.  

- Application 클래스에 `@EnableAsync`로 비동기 기능을 활성화
- 비동기로 동작을 원하는 public 메소드에 `@Async` 어노테이션 설정

그런데 단순히 이 방식으로 할 경우 요청마다 매번 새로운 Thread를 생성하게 된다.  

따라서 Pool에 정해진 갯수만큼 Thread를 미리 생성해두고 필요할 때마다 사용할 수 있도록 따로 구현이 필요하다.  

아래와 같은 방법으로 가능하다.  

- ThreadPoolTaskExecutor로 비동기로 호출하는 Thread에 대한 설정

이 부분들에 대해 조금 자세히 알아보자.  



## `@EnableAsync`

앞서 말한대로 Application 클래스에 `@EnableAsync` 어노테이션을 추가해주기만 하면 되지만, 다음과 같이 다른 옵션들도 설정해줄 수 있다.  

- annotation: 기본으로 Spring의 `@Async` 어노테이션을 감지하지만, 사용자 정의된 다른 어노테이션도 감지할 수 있음
- mode: 사용할 advice 타입을 가리킴
- proxyTargetClass: 사용할 proxy 타입을 가리킴
- order: AsyncAnnotationBeanPostProcessor 가 적용할 순서를 설정



## `@Async`

앞서 말한대로 public 메소드에 `@Async` 어노테이션을 추가해주면 되는데,  

여기서 전제 조건이었던 public 메소드가 `@Async`의 제약조건이다.  

그 이유는 위에서 살펴봤듯이 Spring의 비동기처리는 프록시를 기반으로 하는데, 메소드가 public이어야만 프록시가 될 수 있기 때문이다.  

또 다른 제약조건으로는 셀프호출은 작동하지 않는다.  

이 이유 역시 Spring의 비동기 처리가 프록시를 기반으로 하기 때문에,  셀프 호출과 같이 프록시를 우회하는 것에서는 작동하지 않게 된다.  

