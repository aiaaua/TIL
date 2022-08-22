# Filter-Interceptor

로그인 관련 세션 체크, 권한체크, 로그, 페이지 인코딩 변환 등과 같이 공통적으로 처리해야하는 업무는  

페이지 마다 작성해야한다면 중복도 많아지고 관리도 쉽지 않다.  

따라서 이런 공통적인 업무는 따로 빼서 관리하는 것이 좋은데, Spring에서 이런 방법은 3가지가 있다.

- Filter
- Interceptor
- [AOP](https://github.com/project-565/plog-study/blob/main/spring/06.aop.md)

AOP에 대해서는 이미 다루었으므로 Filter와 Interceptor에 대해서만 알아보자.  

![image](https://user-images.githubusercontent.com/55227276/185053957-4af2f26d-085e-4f60-8fd4-e8de2764f3fa.png)



## Filter

![image](https://user-images.githubusercontent.com/55227276/185060454-4e585386-43f8-470a-9da2-3eb3ac06c3eb.png)

Filter는 Web Application에서 관리되는 영역에 위치하므로 Spring과 무관한 자원에 대해 동작한다.  

(일반적으로 네트워크 통신 기반에 의해 사용되는 것들)  

Filter는 특정 요청과 Controller에 관계없이 전역적으로 처리해야하는 작업이나 웹 어플리케이션에 전반적으로 사용되는 기능을 구현할 때 적용한다.  

Spring Boot Framework에서 Client로부터 오는 요청/응답에 대해서 최초/최종 단계의 위치에 존재한다.  

이를 통해 요청/응답의 정보를 변경하거나 Spring에 의해서 데이터가 변환되기 전의 순수한 Client의 요청/응답 값을 확인할 수 있다.  

`@ControllerAdvice`나 `@ExceptionHandler`를 사용하여 예외처리를 할 수 없으며,   

예외처리가 필요할 경우 `doFilter()` 메소드 주변을 try~catch 구문으로 감싸서 예외를 핸들링한다.  

![image](https://user-images.githubusercontent.com/55227276/185073081-2d221f14-0c07-4f28-9051-59d715d3c768.png)

유일하게 ServletRequest, ServletResponse의 객체를 변환할 수 있다.  

주로 Spring Framework에서는 인증과 관련된 공통작업이나 이미지/데이터 압축 및 문자열 인코딩, request/response의 Logging을 하는데 사용한다.  

이를 선/후 처리함으로써 Service business logic과 분리시킨다.  

### 실행 메서드

Filter의 실행 메서드는 다음과 같이 세 가지가 있다.  

- `init()`: 필터 인스턴스 초기화
- `doFilter()`: 전/후 처리
- `destroy()`: 필터 인스턴스 종료

`init()`과 `destroy()`는 디폴트 메소드이므로 필요에 따라 구현하면 된다.  
