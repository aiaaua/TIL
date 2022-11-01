# Exception

Spring에서 Exception 처리는 크게 네 가지 방식이 있다.

- `@ResponseStatus`

- `@ExceptionHandler`
- `@ControllerAdvice` & `@RestControllerAdvice`
- `@ResponseStatusException`

이 중에서 `@ExceptionHandler`와  `@ControllerAdvice` & `@RestControllerAdvice`두 가지의 사용법을 알아보자.  



## `@ExceptionHandler`

Web계층의 Controller에서 발생하는 에러를 처리해준다.  

(Service, Repository계층에서 발생하는 에러는 제외)  

`@Controller`로 선언된 클래스 내부의 메소드에 `@ExceptionHandler` 어노테이션을 붙여주면 되며,  

value값으로 특정 Exception을 설정해주면 된다.  

주의해야할 점은 `@ExceptionHandler`에 등록된 예외 클래스와 파라미터로 받는 예외 클래스가 동일해야한다는 것이다.  

(만약 값이 다르면 런타임시 에러 발생됨)  

```java
@RestController
@RequiredArgsConstructor
public class ProductController {

    ...

    @ExceptionHandler(NoSuchElementFoundException.class)  // value값으로 Exception 설정
    public ResponseEntity<ErrorResponse> handleItemNotFoundException(NoSuchElementFoundException exception) {
        ...
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleMethodArgumentNotValid(MethodArgumentNotValidException ex) {
        ...
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleAllUncaughtException(Exception exception) {
        ...
    }
}
```

`@ExceptionHandler`의 장점은 특정 Controller에서 발생하는 에러에 대해 처리하기 좋고, 에러 응답을 자유롭게 다룰 수 있다는 것이다.  

그러나 처리하고자 하는 예외가 여러 Controller에서 발생하는 예외일 경우 에러 처리 코드가 중복될 수 있다는 단점이 있다.   

이런 경우에는 `@ControllerAdvice` & `@RestControllerAdvice`방법을 이용하는 것이 좋다.  



## `@ControllerAdvice` & `@RestControllerAdvice`

`@ExceptionHandler`와 동일하게 Web계층의 Controller에서 발생하는 에러를 처리할 수 있으며,   

특정 package의 예외처리나, Global 예외처리도 가능하다.  

`@RestControllerAdvice`는 `@ControllerAdvice`와 기본적으로 동일하지만, `@ResponseBody`가 붙어있어 응답을 JSON형태로 내려줄 수 있다.  

(`@ControllerAdvice`는 HTML 뷰로 값을 리턴함)  

`@ControllerAdvice`나 `@RestControllerAdvice` 안에서 `@ExceptionHandler` 을 붙여서 사용한다.  

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(NoSuchElementFoundException.class)
    protected ResponseEntity<?> handleNoSuchElementFoundException(NoSuchElementFoundException e) {
        final ErrorResponse errorResponse = ErrorResponse.builder()
                .code("Item Not Found")
                .message(e.getMessage()).build();

        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(errorResponse);
    }
}
```

`@ControllerAdvice`나 `@RestControllerAdvice`는 특정 Controller가 아닌 모든 Controller에서 동일하게 적용된다.  

따라서 특정 클래스에만 제한적으로 적용하고 싶다면 다음의 세 가지 방법을 사용할 수 있다.  

```java
// 1. annotations 지정
@ControllerAdvice(annotations = RestController.class)
public class ExampleAdvice1 {}

// 2. basePackages 또는 basepackagesClasses 지정(탐색 패키지 또는 탐색 클래스)
@ControllerAdvice("org.example.controllers")
public class ExampleAdvice2 {}

// 3. assignableTypes 지정
@ControllerAdvice(assignableTypes = {ControllerInterface.class, AbstractController.class})
public class ExampleAdvice3 {}
```

`@ControllerAdvice`나 `@RestControllerAdvice`를 사용할 때 주의사항은 다음과 같다.  

1. 한 프로젝트당 하나의 `@ControllerAdvice`나 `@RestControllerAdvice`만 관리하는 것이 좋음  
2. 만약 여러 개가 필요하다면 annotations나 basePackages/basepackageClass 등을 지정  
3. 여러 `@ControllerAdvice`나 `@RestControllerAdvice`가 있다면 `@Order`로 순서 지정(미지정시 임의의 순서로 에러 처리됨)
4. 구현한 Exception 클래스들을 한 공간에서 관리 
