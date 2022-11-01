# Server 기본동작

HTTP의 요청을 특정하는 8가지 method는 아래와 같다.  

|         | 의미                             | CRUD  | 멱등성 | 안정성 | Path Variable | Query Parameter | DataBody |
| ------- | -------------------------------- | ----- | ------ | ------ | ------------- | --------------- | -------- |
| GET     | 리소스 취득                      | R     | O      | O      | O             | O               | X        |
| POST    | 리소스 생성, 추가                | C     | X      | X      | O             | △               | O        |
| PUT     | 리소스 갱신, 생성                | C / U | O      | X      | O             | △               | O        |
| DELETE  | 리소스 삭제                      | D     | O      | X      | O             | O               | X        |
| HEAD    | 헤더 데이터 취득                 | -     | O      | O      | -             | -               | -        |
| OPTIONS | 지원하는 메소드 취득             | -     | O      | -      | -             | -               | -        |
| TRACE   | 요청 메세지 반환                 | -     | O      | -      | -             | -               | -        |
| CONNECT | 프록시 동작의 터널 접속으로 변경 | -     | X      | -      | -             | -               | -        |

이 8가지 method 중 기본동작인 GET, POST, PUT, DELETE을 Spring Boot로 사용하는 방법에 대해 알아보자.  



## RequestMapping

client의 요청을 처리하기 위한 api url을 매핑할 때 Spring의 `@RequestMapping`이라는 어노테이션을 사용한다.

`@RequestMapping` 의 동작원리부터 옵션까지 알아보자.  

### 동작원리

> 사실 이 부분은 잘 이해하지는 못했어서 태영님이 내용을 같이 봐주시면 좋겠습니다.

`@RequestMapping`과 관련된 빈의 이름은 `RequestMappingHandlerMapping`이다.  

이름에서 알 수 있듯이 RequestMapping과 Handler를 매핑해주는 빈이다.  

`RequestMappingHandlerMapping` 빈의 초기화 전체 흐름은 다음과 같다.  

1. RequestMappingHandlerMapping 빈 생성
2. 빈 초기화 하면서, initHandlerMethods 호출
3. 빈 팩토리에 등록되어 있는 빈들 중 `@Controller` 또는 `@RequestMapping` 를 가지고 있는 빈을 가져옴
4. 핸들러가 될 수 있는 모든 메서드를 추출(detectHandlerMethods)
5. 추출된 메서드를 registry에 등록(registerHandlerMethod)

 `RequestMappingHandlerMapping`빈은 `@RequestMapping`으로 등록한 메서드들을 가지고 있다가 요청이 들어오면 빈으로 등록된 `HandlerMapping`들이 변수를 찾아서 매핑해주는 역할을 한다.   

`@RequestMapping`은 요청이 들어오면 컨트롤러와 매핑해주고, 컨트롤러를 실행시켜 응답을 받는 방식으로 동작한다.  

### 옵션

`@RequestMapping`은 `value`, `method`, `params`, `headers`, `consumes`, `produces` 라는 옵션을 줄 수 있으며,  

이 옵션들은 handler에서 매핑하는 기준이 되므로 url과 모든 옵션이 같을 수 없다.  

#### value

value는 연결할 url을 지정하는 옵션으로 다음과 같이 설정할 수 있다.

```java
@RequestMapping(value = "/example")
@RequestMapping("/example")  // value외에 다른 옵션이 주어지지 않았을 경우
```

보통 호스트 주소와 포트 번호를 제외하고 `API설계 규약`에 따라 이름을 짓는다.  

이 때 API 설계규약이란 RESTful API에서의 URL Rules을 의미하는 것으로 다음과 같은 것들이 해당된다.  

1. 마지막에 / 를 포함하지 않는다.
2. _ 대신 - 를 사용한다.
3. 소문자를 사용한다.
4. method는 URL에 포함하지 않는다.

이는 이미 Web에서 살펴본 내용이므로 자세한 것은 [Web 문서](https://github.com/project-565/plog-study/blob/main/spring/02.web.md#설계원칙)를 참고하자.  

또한 `Ant Style Pattern`을 적용해 url mapping이 가능하며, `{}` 사이에 변수를 넣어 url을 매핑하는 것도 가능하다.

```java
@RequestMapping("/example/**")
@RequestMapping("/example/{id}")
```

여기서 `Ant Style Pattern`은 `?` , `*`, `**` 와 같은 와일드 카드 문자를 사용해 패턴을 매핑하는 것으로 정규표현식과 유사하다.  

Spring에서는 `AntPathMatcher`에 구현되어있는데, 이는 Apach Ant Style에서 따온 것이며 `PathMatcher`를 구현하여 resource와 path를 매핑하는 기술이다.  

각 와일드 카드는 아래와 같은 것을 의미한다.  

- `?` : 하나의 문자와 일치
- `*` : 0개 이상의 문자와 일치
- `**` : 경로에서 0개 이상의 디렉토리 와 일치



#### method

method에는 RequestMethod로 명명되어있는 `GET`, `HEAD`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS`, `TRACE` 의 8가지 method를 사용하는 것이 일반적이며 아래와 같이 사용할 수 있다.  

```java
@RequestMapping(method = RequestMethod.GET, value = "/example")
```

특히 Spring 4.3부터 이 8개의 method 중에서 `GET`, `POST`, `PUT`, `PATCH`, `DELETE`에 대해서는 각각에 맞는 method 옵션이 적용된 어노테이션이 존재하여 다음과 같이 사용할 수도 있다.  

```java
@GetMapping("/example")
```

`@RequestMapping` 어노테이션을 사용하는 것보다 각 method 옵션이 적용된 어노테이션을 사용하는 것이 많이 사용되는 방법이다.  

(이 후 예제도 이 방법으로 작성한 것들을 살펴볼 예정이다.)  

물론 이 방법이 훨씬 간결하고 가독성이 좋기도 하지만, 단순히 이 이유때문에 method 옵션 어노테이션을 사용하는 것은 아니다.  

왜냐하면 `@RequestMapping`의 경우 앞서 value에서 말한 것처럼 method 부분을 생략하고 value만 명시해서 사용할 경우 충분히 간결하게 사용할 수 있으며, 이렇게 method를 생략하면 GET 방식과 POST 방식을 모두 처리해주므로 어떻게 보면 이것이 더 편하기 때문이다.  

그렇다면 이렇게 method가 명시된 어노테이션을 사용하는 이유는 뭘까? 이유는 크게 두 가지가 있다.  

1.  url의 중복 사용 가능

   url 요청을 통해 메서드를 요청할 때 전송방식을 명시하면 하나의 url로도 두 개 이상의 매핑을 처리할 수 있다. 

   ```java
   @getMapping("/example")
   @PostMapping("/example")
   ```

   만약 `@RequestMapping`에서 특정 url로 GET방식의 요청을 받았다면 같은 url로 POST방식의 요청을 받지 못한다.

   따라서 url을 중복 사용함으로써 경제적이며 코드 관리에도 더욱 유리하다고 할 수 있다.

2. 코드에 의미 명시

   method가 명시되어있다면 코드만 봐도 어떤 전송 방식을 처리하는지 확인할 수 있다.  



#### params

params는 api url에 파라미터를 전달하고 싶을 때 사용하는 옵션이다.  

params에 전달한 것과 일치하는 param이 붙으면 이 메서드로 매핑해준다.  

```java
@RequestMapping(method = RequestMethod.GET, value = "/example", params = {"id", "password"})
public ResponseEntity<Example> getExample(@RequestParam("id") int paramId, @RequestParam("password") String password) {
    Example example = new Example("example" + paramId + password);
    return ResponseEntity.ok(example);
}
```



#### headers

headers는 사용자가 요청하는 header에 특정 header가 존재하도록 제한할 때 사용할 수 있다.  

배열로 값을 줄 수있으며, `!`, `=` 등 문자열처럼 이용할 수 있다.  

전달할 수 있는 header의 종류는 `Content-Type`등 [여러가지](https://developer.mozilla.org/ko/docs/Web/HTTP/Headers)가 있다.  

```java
@RequestMapping(method = RequestMethod.POST, value = "/example/{text}", headers = "content-type=text/plain")
public ResponseEntity<Example> getExample3(@PathVariable("text") String text) {
    Example example = new Example("example" + text);
    return ResponseEntity.ok(example);
}
```



#### consumes

consumes는 request의 content-type request header가 일치하는 옵션을 찾는 옵션이다.  

수신하고자 하는 데이터 포맷을 정의하여 사용자가 `@RequestBody`에 담는 타입을 제한할 수 있다.  

필수 옵션이 아니며, `@RequestBody`와 `@ModelAttribute`는 `application/json`이 기본값으로, `@RequestParam`, `@PathVariable`은 `text/plan`이 기본값으로 전달된다.  

```java
@RequestMapping(method = RequestMethod.POST, value = "/produce{example}", consumes = MediaType.TEXT_PLAIN_VALUE)
public ResponseEntity<Example> getExample5(@PathVariable String example) {
    Example example1 = new Example(example);
    return ResponseEntity.ok(example1);
}
```



#### produces

produces는 response의 accept request header가 특정 옵션으로 반환될 것을 지정하는 옵션이다.  

출력하고자 하는 데이터 포맷을 정의하여 data Type을 지정하면 해당 data Type으로만 사용자에게 응답할 수 있다.  

필수 옵션이 아니며, 기본값으로 `application/json`으로 전달된다.  

```java
@RequestMapping(method = RequestMethod.GET, value = "/test", produces = MediaType.TEXT_PLAIN_VALUE)
public String getExample() {
    return "example";
}
```



## Reference

- https://tecoble.techcourse.co.kr/post/2021-06-18-spring-request-mapping/
- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/AntPathMatcher.html
- https://velog.io/@codren/AntMatchers
- https://change-words.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-RequestMapping-%EB%8C%80%EC%8B%A0-PostMapping-GetMapping-%EC%93%B0%EB%8A%94-%EC%9D%B4%EC%9C%A0
- https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html
- https://2ham-s.tistory.com/292
- https://pplenty.tistory.com/7
- https://pplenty.tistory.com/6
