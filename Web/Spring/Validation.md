# Validation

DTO나 객체를 검증해야하는 경우 `@Valid`, `@Validated` 어노테이션을 활용하여 검증을 쉽게 할 수 있다.   

`@Valid`는 Java에서 지원해주는 어노테이션이며 `@Validated`는 Spring에서 지원해주는 어노테이션이다.   

따라서 `@Validated`는 `@Valid`의 기능을 포함하고 있으며, 유효성을 검토할 그룹을 지정할 수 있는 기능을 추가로 가지고 있다.    



##  Validation 기본

### 의존성 추가

Spring Boot 2.3 이전에는 `spring-boot-starter-web`의존성에 validation이 있었지만,   

Spring Boot 2.3 부터는 모듈로 빠졌기 때문에 따로 추가해주어야 사용이 가능하다.  

```java
// Gradle
implementation 'org.springframework.boot:spring-boot-starter-validation'
```



### Validation 어노테이션 종류

여러가지 validation 어노테이션을 제공하고 있는데, 아래 작성된 예시 외의 어노테이션은 [여기](https://docs.jboss.org/hibernate/validator/6.2/reference/en-US/html_single/#validator-defineconstraints-spec)에서 확인 가능하다.  

| 어노테이션           | 설명                                                |
| -------------------- | --------------------------------------------------- |
| @Null                | null만 허용함                                       |
| @NotNull             | 빈 문자열, 공백은 허용하되 null은 허용하지 않음     |
| @NotEmpty            | 공백은 허용하되 null과 빈 문자열은 허용하지 않음    |
| @NotBlank            | null, 빈 문자열, 공백을 모두 허용하지 않음          |
| @Email               | 이메일 형식을 검사함(단, 빈 문자열의 경우 통과시킴) |
| @Pattern(regexp = )  | 정규식 검사할 때 사용                               |
| @Size(min =, max =)  | 길이를 제한할 때 사용                               |
| @Max(value = )       | value 이하의 값만 허용                              |
| @Min(value = )       | value 이상의 값만 허용                              |
| @Positive            | 값을 양수로 제한                                    |
| @PositiveOrZero      | 값을 양수와 0만 가능하도록 제한                     |
| @Negative            | 값을 음수로 제한                                    |
| @NegativeOrZero      | 값을 음수와 0만 가능하도록 제한                     |
| @Future              | Now 보다 미래의 날짜, 시간이어야 함                 |
| @FutureOrPresent     | Now 거나 미래의 날짜, 시간이어야 함                 |
| @Past                | Now 보다 과거의 날짜, 시간이어야 함                 |
| @PastFutureOrPresent | Now 거나 과거의 날짜, 시간이어야 함                 |
| @Valid               | 해당 object validation 실행                         |



### Validation으로 할 수 있는 것

크게 다음과 같은 세 가지 validation을 수행할 수 있다.  

- Spring MVC Controller
- Spring Service
- Spring Repository

이 중에서 Spring MVC Controller에 대해서 조금 더 자세히 알아보자.  



#### Spring MVC Controller의 Validation

Spring RestController를 통해 Client로부터 전달받은 요청(request, input)에 대해 유효성을 검증할 수 있다.  

##### `@RequestBody`

`@RequestBody` 어노테이션을 사용하고 있는 매개변수에 `@Valid` 어노테이션을 함께 설정하면  

`@RequestBody`로 전달받은 값에 대해 validation이 가능하다.  

```java
@Slf4j
@RestController
@RequiredArgsConstructor
public class ProductRestController {

	@PostMapping(value = "/products")
	public ResponseEntity<Void> create(@Valid @RequestBody ProductRequest request) {
		log.info("productRequest: {}", request);
		return ResponseEntity.noContent().build();
	}
}
```

`@RequestBody`처리를 위해 데이터 바인딩 중 validation에 오류가 있을 경우 `MethodArgumentNotValidException` 오류가 발생한다.  

이는 Spring에서 기본적으로 HTTP status code 400으로 변환되는 exception이다.  



##### `@RequestParam`

파라미터 값에 대해 validation하기 위해서는 먼저 클래스에 `@Validated` 어노테이션을 적용하고  

`@RequestParam`을 설정할 때 `@Valid` 어노테이션을 같이 설정하면 된다.  

```java
@Validated
@RestController
public class ProductRestController {

	@GetMapping(value = "/products")
	public ResponseEntity<Void> search(
			@Min(1) @RequestParam(value = "page") int page,
			@Min(1) @Max(100) @RequestParam(value = "size") int size,
			@Range(min = 1, max = 10) @RequestParam(value = "keyword") String keyword) {
		// page는 1보다 크고 size는 1~100 사이
		// keyword는 글자수 1~10 사이
		return ResponseEntity.noContent().build();
	}
}
```

파라미터 값에 문제가 있을 경우 `ConstraintViolationException` 오류가 발생한다.  

이는 Spring에서 기본적으로 handling해주는 exception이 아니기 때문에 HTTP status code 500으로 처리된다.  



##### `@PathVariable`

리턴 값에 대해 validation하기 위해서는 먼저 클래스에 `@Validated` 어노테이션을 적용하고  

`@PathVariable`을 설정할 때 `@Valid` 어노테이션을 같이 설정하면 된다.  

```java
@Validated
@RestController
public class ProductRestController {
    
	@GetMapping(value = "/products/{productNo}")
	public ResponseEntity<Void> getProduct(
			@Min(1) @PathVariable("productNo") int productNo) {
		// productNo는 최소 1이상
		return ResponseEntity.noContent().build();
	}
}
```

리턴 값에 문제가 있을 경우 `ConstraintViolationException` 오류가 발생한다.  

이는 Spring에서 기본적으로 handling해주는 exception이 아니기 때문에 HTTP status code 500으로 처리된다.  

