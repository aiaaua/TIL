## Web Application Architecture
JSP를 이용하여 구성할 수 있는 architecture는 `Model1`과 `Model2`로 나뉨  

### Model1
프로젝트를 `JSP(Controller + View)`와 `Java Bean(Model)`로 구성하여 개발하는 형태  
각 static 파일이 연결되면서 동작하므로 선형적이고 직관적이며 개발시간이 비교적 짧음  
로직(Controller)과 화면(View)이 통합되어있어 유지보수가 어려움  
따라서 규모가 작고 개발 후 유지보수가 거의 필요없는 프로젝트에 적합함  
![model1](https://user-images.githubusercontent.com/55227276/177910336-754119da-1264-4203-9374-947cf001b505.png)


#### 동작순서
1. 클라이언트가 웹 브라우저를 통해 `.jsp`파일 요청
2. 웹 서버가 요청을 받아서 `.jsp`에 대한 요청을 `servlet(JSP) Container`로 전달
3. 해당 `JSP`파일을 실행
4. `JSP`와 `Java Bean`을 사용하여 클라이언트에게 response를 위한 `html 문서`를 구성
5. 요청이 왔던 곳으로 response


### Model2
프로젝트를 `Model`, `View`, `Controller`의 세 가지 요소로 모듈화하여 개발하는 개발형태의 일종, `MVC Pattern`이라고도 불림   
현재 Spring Framework가 사용하는 기본적인 구조이며 `View`부분이 따로 모듈화 되어있기 때문에 협업에 유리함   
프로젝트의 형태가 굉장히 복잡해질 수 있으며 개발 시간이 오래 걸림  
따라서 규모가 크고 개발 후에도 주기적으로 유지보수가 필요한 프로젝트에 적합함  
![model2](https://user-images.githubusercontent.com/55227276/177910388-3632dafe-a521-42d8-b7be-893a93bd55b2.png)


#### 동작순서
1. 클라이언트가 웹 브라우저를 통해 `.jsp`파일 요청
2. 웹 서버가 요청 처리를 위해서 해당 페이지를 찾아 `Web Container`에 전달
3. `Servlet(Controller)`이 응답
4. 이에 필요한 `Java Bean`을 불러서 데이터를 가져옴
5. 데이터를 이용하여 `View`와 연결
6. 생성한 웹 페이지를 웹 서버쪽으로 전송
7. 웹 서버가 전송받은 웹 페이지를 클라이언트에게 전송

#### MVC Pattern
MVC Pattern은 프로젝트에서 서로 다른 역할을 수행하는 `Model`, `View`, `Controller`를 각 파트로 모듈화하여 개발하는 형태임  
|요소|역할|
|---|---|
|Model|데이터베이스와 관계된 내용을 기술<br /> 클라이언트의 요청에 의해 필요한 자료를 데이터베이스에서 가져오거나 수정하여 Controller로 전달|
|View|사용자에게 보여지는 UI 내용을 기술<br /> 어떤 View가 보여질지는 Controller에 의해 결정됨|
|Controller|전체적인 통제를 담당<br /> 클라이언트의 요청을 받고 적절한 Model에게 동작을 지시하며 Model이 처리하여 반환한 데이터를 적절한 View로 전달|




### Reference
https://juyoungit.tistory.com/119
