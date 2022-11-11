# Constructor

클래스를 가지고 객체를 생성하면, 해당 객체는 메모리에 즉시 생성되지만 초기화되지 않은 상태임  

사용자가 원하는 값으로 인스턴스 변수를 초기화하려면 초기화만을 위한 public 메서드가 필요함  

이를 간단히 하기 위해 java에서는 생성자(Contsructor)라는 메서드를 제공  

생성자 메서드는 객체의 생성과 동시에 인스턴스 변수르 원하는 값으로 초기화할 수 있음  

생성자의 이름은 해당 클래스 이름과 같아야 하며 다음과 같은 특징이 있음  

- 반환값이 없지만 void형으로 선언하지 않음  
- 초기화를 위한 데이터를 인수로 전달받을 수 있음  
- 초기화 방법이 여러 개 존재할 경우 하나의 클래스가 여러 생성자를 가질 수 있음(메서드 오버로딩 가능)  

## 생성자 선언

매개변수가 없는 생성자는 아래와 같이 선언할 수 있으며

```
CLASS_NAME() {...}
```

매개변수가 있는 생성자는 아래와 같이 선언할 수 있음

```
CLASS_NAME(인수1, 인수2, ...) {...}
```

## 생성자 호출

`new` 키워드를 사용해 객체를 생성할 때 자동으로 생성자가 호출됨







## Reference

- http://www.tcpschool.com/java/java_methodConstructor_constructor

- https://kephilab.tistory.com/47
- https://dingue.tistory.com/14
- https://www.daleseo.com/lombok-popular-annotations/

