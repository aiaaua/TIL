# Redirect와  Forward

redirect와 forward는 `UrlBasedViewResolver`의 `createView` 메소드를 통해  

전달받은 String을 `RedirectView`나 `InternalResourceView`로 만들어서 리턴한다.  



## Redirect

redirect란 웹 브라우저가 어떤 URL로 웹 서버를 요청했을 때, 다른 URL로 넘겨주는 것을 말한다.   

redirect는 서버에 요청이 들어온 후, 서버에서 클라이언트로 HTTP Status Code 302를 전송한다.

_([HTTP Status Code 302](https://developer.mozilla.org/ko/docs/Web/HTTP/Status/302): 클라이언트가 요청한 리소스가 헤더에 주어진 URL에 일시적으로 이동되었음을 가리킴)_   

HTTP Status Code 302를 확인하고 응답의 Location 헤더에서 URL을 읽어들이며,  해당 URL로 다시 요청을 보낸다.  

이동한 URL에서 새로 request와 response객체가 생성되므로 최초의 요청 정보는 유효하지 않게 된다.  

URL이 변화해야할 때 redirect를 쓰는 것이 유리하다.  

예를 들면, mail함에 접속했을 때 로그인이 되어있지 않다면, 로그인을 선행하기 위해 로그인 페이지로 이동시키는 것이 redirect가 사용될 수 있는 예제이다.  

### Example

사용법은 `redirect:` 을 사용하며 `redirect:`일 때는 현재 경로를 기준으로 상대 경로를 사용하며

`redirect:/`처럼 `/`를 붙일 경우 웹 어플리케이션을 기준으로 이동경로를 생성한다.  

```java
@Controller
public class RegisterController {

	@RequestMapping("/register/step1")
	public String handleStep1() {
		return "redirect:/register/step1";
	}

	@PostMapping("/register/step2")
	public String handleStep2(
			@RequestParam(value = "agree", defaultValue = "false") Boolean agree,
			Model model) {
		if (!agree) {
			return "redirect:/register/step1";
		}
		model.addAttribute("registerRequest", new RegisterRequest());
		return "redirect:/register/step2";
	}

}
```



## Forward

forward는 Web Container 차원에서 동일한 웹 컨테이너의 페이지로 이동을 말한다.  

단, 실제 웹 브라우저에서는 다른 페이지로 이동했음을 알 수 없다.   

다음으로 이동하는 URL에 요청 정보를 그대로 전달하기 때문에 최초 요청 정보는 다음 URL에서도 유효하다.  

객체를 재사용하거나 공유해야할 때 forward를 사용하는 것이 유리하다.  

예를 들어, 휴대전화 번호가 바뀌게 되었으나 이전 번호로 전화했을 때 새로운 번호로 바로 연결시켜주는 것이 forward가 사용될 수 있는 경우이다.  

### Example

```java
@RestController
@RequiredArgsConstructor
public class ForwardController {

    @GetMapping("/forward/origin")
    public String forward() throws URISyntaxException {
        URI forwardUri = new URI("https://www.daum.net");
        RestTemplate restTemplate = new RestTemplate();
        return restTemplate.getForObject(forwardUri, String.class);
    }
}
```



## Reference

- https://jaehoney.tistory.com/48
- https://blog.junu.dev/39
- https://iamyourfavorite.tistory.com/157
- http://jonggs.blogspot.com/2018/11/spring-redirect-forward.html
- https://effectivesquid.tistory.com/entry/Redirect-%EC%99%80-Forward