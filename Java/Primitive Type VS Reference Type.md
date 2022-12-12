# Primitive Type VS Reference Type

Java의 데이터 타입은 `Primitive Type(기본 타입)`과 `Reference Type(참조 타입)` 두 가지로 나뉨  

![image](https://user-images.githubusercontent.com/55227276/206993885-458336b5-cb0e-4a1b-80f8-37bf03af6e5e.png)



## Primitive Type

`Primitive Type`은 stack 메모리에 값이 존재함  

비객체타입이기 때문에 Null값을 가질 수 없음  

값을 필요로하여 호출할 때 바로 값에 접근할 수 있어 접근 속도가 빠름  

`Reference Type`보다 사용하는 메모리 양이 적음  

컴파일 시점에 데이터의 표현 범위를 벗어나면 컴파일 에러가 발생함  

generic 타입으로 사용할 수 없음  
_(generic이란 데이터 타입을 일반화하는 것을 의미함)_  


|Type   |Memory|Default|Data                                                  |
|-------|------|-------|------------------------------------------------------|
|boolean|1byte |false  |true, false                                           |
|byte   |1byte |0      |-128 ~ 127                                            |
|short  |2byte |0      |-32,768 ~ 32,767                                      |
|int    |4byte |0      |-2,147,483,648 ~ 2,147,483,647                        |
|long   |8byte |0L     |-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807|
|float  |4byte |0.0F   |(3.4 * 10^(-38)) ~ (3.4 * 10^38)                      |
|double |8byte |0.0    |(1.7 * 10^(-38)) ~ (1.7 * 10^38)                      |
|char   |2byte |\u0000 |0 ~ 65,535                                            |



## Reference Type

`Reference Type`은 stack 메모리에는 객체의 참조값만 있고 heap 메모리에 실제 값이 존재함  

기본적으로 `java.lang.Object`를 상속받기 때문에 객체타입이며 Null값을 가질 수 있음  

`Primitive Type`을 `Reference Type`으로 변환시키는 것을 `Boxing`이라고 하며 반대를 `Unboxing`이라고 함  

값을 필요로하여 호출할 때마다 `Unboxing` 과정을 거치므로 `Primitive Type`보다 접근 속도가 느림  

|Type       |Memory|Default|
|-----------|------|-------|
|array      |4byte |null   |
|enumeration|4byte |null   |
|class      |4byte |null   |
|interface  |4byte |null   |

![image](https://user-images.githubusercontent.com/55227276/207061055-8f6de382-d27c-4f8b-81e0-433c803b40f2.png)


## Reference

- https://n1tjrgns.tistory.com/264
- https://bangu4.tistory.com/32
- https://itstarter.tistory.com/793
- https://yeastriver.tistory.com/7
- https://velog.io/@gillog/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85Primitive-Type-Reference-Type
- https://coder-in-war.tistory.com/entry/Java-13-Java%EC%9D%98-%EC%9E%90%EB%A3%8C%ED%98%95-Primitive-type-Reference-type
