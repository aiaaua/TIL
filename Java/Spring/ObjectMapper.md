# ObjectMapper

ObjectMapper는 Jackson 라이브러리의 클래스로 JSON형식의 응답을 직렬화(serialize) 혹은 역직렬화(deserialize)할 때 사용한다.   

크게 다음과 같이 세 가지 메서드가 있으며 

- `readValue()`: JSON String을 Java Object로 변환
- `convertValue()`: Java Object를 또 다른 Java Object로 변환
- `writeValueAsString()`: Java Object를 JSON String으로 변환

이 세 메서드를 통해 다음의 5가지 변환에 대해 예제로 알아보자.  

- Java Object -> JSON String
- JSON String -> Java Object
- JSON -> Jackson JsonNode
- JSON Array String -> Java List
- JSON String -> Java Map



## 예제

라이브러리 사용에 앞서 다음과 같이 종속성을 추가하고

```java
implementation 'com.fasterxml.jackson.core:jackson-databind:2.12.3'
```

다음과 같은 Java Object가 있다고 하자.  

```java
public class User {

    private String name;
    private int age;
    
    public User(String name, int age) {
    	this.name = name;
        this.age = age;
    }
    
    public String getName() {
    	return name;
	}
    
    public int getAge() {
    	return age;
	}
}
```

그리고 다음과 같이 ObjectMapper와 User를 선언하자.   

```java
ObjectMapper objectMapper = new ObjectMapper();
User user = new User("Ryan", 30);
```



### Java Object -> JSON String

```java
objectMapper.writeValue(new File("user.json"), user);

// 파일 출력: user.json
{"name":"Ryan","age":30}

// 문자열 출력
String userAsString = objectMapper.writeValueAsString(user);
{"name":"Ryan","age":30}
```



### JSON String -> Java Object

```java
String json = "{ \"name\" : \"Ryan\", \"age\" : 30 }";
User user = objectMapper.readValue(json, User.class);

// JSON File to Object
User user = objectMapper.readValue(new File("user.json"), User.class);

// JSON URL to Object
User user = objectMapper.readValue(new URL("file:user.json"), User.class);
```



### JSON -> Jackson JsonNode

```java
String json = "{ \"name\" : \"Ryan\", \"age\" : 30 }";
JsonNode jsonNode = objectMapper.readTree(json);

// name: Ryan
String name = jsonNode.get("name").asText();

// age: 30
int age = jsonNode.get("age").asInt();
```



### JSON Array String -> Java List

```java
String jsonArr = "[{\"name\":\"Ryan\",\"age\":30},{\"name\":\"Jake\",\"age\":20}]";
List<User> users = objectMapper.readValue(jsonArr, new TypeReference<>() {});
```



### JSON String -> Java Map

```java
String jsonArr = "{\"name\":\"Ryan\",\"age\":30}";
Map<String, Object> user = objectMapper.readValue(jsonArr, new TypeReference<>() {});
```



## Reference

- https://blog.naver.com/PostView.naver?blogId=occidere&logNo=222512549848&redirect=Dlog&widgetTypeCall=true&directAccess=false
- https://escapefromcoding.tistory.com/341
- https://interconnection.tistory.com/137

