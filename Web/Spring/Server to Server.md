# Server to Server

웹 어플리케이션에서 요청자가 제공자에게 무언가를 요청할 때 프로그램에서 가장 흔히 사용하는 것이 바로 `HTTP Client` 이다.  

Spring에서 `HTTP Client`로 제공하는 것은 `RestTemplate`와 `WebClient` 등이 있는데,  

두 가지의 차이점은 Blocking/Non-blocking, Synchronouse/Asunchronous 지원여부 등이다.  

(Blocking/Non-blocking, Synchronouse/Asunchronous에 대해 설명을 보고싶다면 [여기](https://gngsn.tistory.com/154)를 참고하자.)  

이 두 가지 모듈에 대해 모두 알아보자.  

## RestTemplate

Spring 3부터 지원되었으며, REST API 호출이후 응답을 받을 때까지 기다리는 동기방식이다.   

Spring에서 제공하는 다른 Template 클래스(JdbcTemplate, RedisTemplate..)와 동일한 원칙에 따라 설계되어있다.  

(그런데 Spring에서는 이제 RestTemplate을 서서히 없앤다고 해서 간단히만 알아볼 예정이다.)  

Blocking 기반의 동기방식을 사용하는 템플릿이며 RESTful 형식에 맞추어져있다.  

### 의존성 추가

다음과 같이 의존성을 추가해주어야한다.  

```java
implementation 'org.apache.httpcomponents:httpcore:4.4.15'
implementation 'org.apache.httpcomponents:httpclient:4.5.13'
```

### 메서드

REST 서비스를 호출하도록 설계되어 HTTP 프로토콜의 메서드에 맞게 여러 메서드를 제공한다.  

| 메서드          | HTTP    | 설명                                                         |
| --------------- | ------- | ------------------------------------------------------------ |
| getForObject    | GET     | 주어진 URL 주소에 HTTP GET 메서드로 결과를 객체로 반환받는다. |
| getForEntity    | GET     | 주어진 URL 주소에 HTTP GET 메서드로 결과를 ResponseEntity로 반환받는다. |
| postForLocation | POST    | POST 요청을 보내고 결과로 헤더에 저장된 URI를 반환받는다.    |
| postForObject   | POST    | POST 요청을 보내고 결과로 객체를 반환받는다.                 |
| postForEntity   | POST    | POST 요청을 보내고 결과로 ResponseEntity를 반환받는다.       |
| delete          | DELETE  | 주어진 URL 주소로 HTTP DELETE 메서드를 실행한다.             |
| headForHeaders  | HEADER  | 헤더의 모든 정보를 얻을 수 있으면 HTTP HEAD 메서드를 사용한다. |
| put             | PUT     | 주어진 URL 주소로 HTTP PUT 메서드를 실행한다.                |
| patchForObject  | PATCH   | 주어진 URL 주소로 HTTP PATCH 메서드를 실행한다.              |
| optionsForAllow | OPTIONS | 주어진 URL 주소에서 지원하는 HTTP 메서드를 조회한다.         |
| exchange        | any     | HTTP 헤더를 새로 만들 수 있고 어떤 HTTP 메서드도 사용 가능하다. |
| execute         | any     | Request/Response 콜백을 수정할 수 있다.                      |

### Create

RestTemplate는 builder를 통해 생성할 수 있다.  

```java
@Configuration
public class RestTemplateClient {

    @Bean
    public RestTemplate restTemplate(RestTemplateBuilder builder) {
        return builder.build();
    }
}
```

### Request and etc

그리고 앞서 살펴본 메서드들을 사용하려면 `restTemplate.[메소드명]()` 와 같이 사용하면 된다.  

아래 예제를 참고하자.  

```java
public class RestTemplateTestClass {
    @Autowired 
    RestTemplate restTemplate;

    public TestClass(RestTemplate restTemplate){
            this.restTemplate = restTemplate;
    }

    public String getSthFromServer(){
        return restTemplate.getForObject("https://example.com",String.class);  // 이 부분처럼 사용
    }
}
```



## WebClient

Spring 5에 추가된 Non-blocking 리액티브 웹 클라이언트로 동기, 비동기 방식을 모두 지원한다.  

### 의존성 추가

Web Client 의존성은 다음과 같이 추가해주면 된다.  

```java
// gradle
dependencies {
    compile 'org.springframework.boot:spring-boot-starter-webflux'
}
```

### Create

Web Client를 생성하는 방법은 2가지가 있으며,  

그 중 `create()`는 디폴트 세팅으로 생성할 수 있는 방법이고, `build()`는 커스터마이즈하여 생성할 수 있는 방법이다.  

```java
WebClient.create();
```

````java
WebClient client = WebClient.builder()
  .baseUrl("http://localhost:8080")
  .defaultCookie("cookieKey", "cookieValue")
  .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE) 
  .defaultUriVariables(Collections.singletonMap("url", "http://localhost:8080"))
  .build();
````

### Request

이에 앞서 리액터의 Mono와 Flux를 알아야하는데 간단히 Mono는 0 또는 하나의 데이터 항목과 에러를 가지며 Flux는 0 또는 하나 이상의 데이터 항목가 에러를 갖는다. (자세한 설명은 [여기](https://github.com/gngsn/Gngsn-Spring-Lab/blob/master/concept-note/Reactor.md#reactor-%EC%A2%85%EB%A5%98)를 참고하자.)  

여기서는 가장 자주 쓰이는 GET과 POST에 대해서 알아보자.  

#### GET

GET API를 사용하여 단일 리소스 혹은 리소스 모음을 가져올 수 있다.  

```java
@Autowired
WebClient webClient;

public Mono<Employee> findById(Integer id) {  // Mono, 단일 리소스
	return webClient.get()
		.uri("/employees/" + id)
		.retrieve()
		.bodyToMono(Employee.class);
}
```

```java
@Autowired
WebClient webClient;

public Flux<Employee> findAll() {  // Flux, 리소스 모음
	return webClient.get()
		.uri("/employees")
		.retrieve()
		.bodyToFlux(Employee.class);
}
```

#### POST

body를 포함하는 POST의 경우 아래와 같이 작성하며  

```java
@Autowired
WebClient webClient;

public Mono<Employee> create(Employee empl) {
	return webClient.post()
		.uri("/employees")
		.body(Mono.just(empl), Employee.class)
		.retrieve()
		.bodyToMono(Employee.class);  // 응답받을 내용에 대한 부분
}
```

body가 없는 POST의 경우 아래와 같이 작성한다.  

```java
@Autowired
WebClient webClient;

public Mono<Void> create(Employee empl) {
	return webClient.post()
		.uri("/employees")
		.body(Mono.just(empl), Employee.class)
		.retrieve()
		.bodyToMono(Void.class);  // Void.class는 body가 없을 때 사용
}
```

### Response

응답을 받는 메소드는 2가지가 있는데,  

그 중 `retrieve()`는 body 를 받아 디코딩하는 간단한 메소드이며, `exchange()`는 ClientResponse를 상태값, 헤더와 함께 가져오는 메소드이다.  

`exchange()`는 세세한 컨트롤이 가능하지만, 메모리 누수 가능성이 있어 `retrieve()`가 권고된다고 한다.  

(`exchange()`는 서서히 없어질 것이므로 예제로 살펴보지 않을 예정이다.)  

`retrieve()`를 사용하여 데이터를 받는 형식은 다시 크게 두 가지가 있는데,  

먼저 `toEntity()` 방식은 아래와 같이 쓰며

```java
Mono<ResponseEntity<Person>> entityMono = client.get()
     .uri("/persons/1")
     .accept(MediaType.APPLICATION_JSON)
     .retrieve()
     .toEntity(Person.class);
```

status, header, body를 포함하는 ResponseEntity를 타입으로 받을 수 있다.  

또 다른 방법인 `toMono()`와 `toFlux()` 방식은 아래와 같이 쓰며  

```java
Mono<Person> entityMono = client.get()
    .uri("/persons/1")
    .accept(MediaType.APPLICATION_JSON)
    .retrieve()
    .bodyToMono(Person.class);
```

body의 데이터만 받을 수 있으므로 필요한 것을 선택해 사용하면 된다.  

### Error Handling

Error Handling도 `retrieve()`에 대해서만 알아보자.   

`retrieve()`는 StatusCode별로 아래와 같이 처리할 수 있다.  

```java
Mono<Person> result = client.get()
        .uri("/persons/{id}", id).accept(MediaType.APPLICATION_JSON)
        .retrieve()
        .onStatus(HttpStatus::is4xxClientError, response -> ...)
        .onStatus(HttpStatus::is5xxServerError, response -> ...)
        .bodyToMono(Person.class);
```



## Reference

- https://blog.advenoh.pe.kr/spring/%EC%8A%A4%ED%94%84%EB%A7%81-RestTemplate/q
- https://blog.naver.com/hj_kim97/222295259904

- https://tecoble.techcourse.co.kr/post/2021-07-25-resttemplate-webclient/
- https://happycloud-lee.tistory.com/220

- https://gngsn.tistory.com/154
- https://velog.io/@yyong3519/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-WebClient
