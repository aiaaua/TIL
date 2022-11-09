# Stream API

`stream`은 컬렉션 혹은 배열에 저장되어있는 요소들을 하나씩 참조하여 반복적인 처리가 가능함  

`stream`은 내부 반복문이기 때문에 for문과 같은 반복문을 사용하지 않더라도 배열 처리를 간단히 할 수 있음  

또한 `stream`은 원본 데이터를 읽기만 하므로 원본 데이터를 변경하지 않음  



## Stream API 생성

### Stream 객체 생성

`Stream.of()` 메서드를 이용해 Stream 객체를 생성할 수 있음  

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4);
```

 ### Stream 객체 변환

`Collection` 인터페이스에 정의되어있는 `stream()` 메서드를 사용하여 배열 등 여러 데이터를 가지는 집합을 `stream` 객체로 변환할 수 있음  

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(1);
list.add(2);
list.add(3);
list.add(4);

Stream<Integer> stream = list.stream();
```



## Stream API 중개 연산

`stream` 중개 연산 메서드는 다음과 같이 여러 가지가 있음  

| Method                                                       | Discription                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Stream<T> **filter**(Predicate<? super T> predicate)         | 해당 stream에서 주어진 조건(predicate)에 맞는 요소만으로 구성된 새로운 stream을 반환함 |
| <R> Stream<R> **map**(Functoin<? super T, ? extends R> mapper) | 해당 stream의 요소들을 주어진 함수에 인수로 전달하여, 그 반환값으로 이루어진 새로운 stream을 반환함 |
| <R> Stream<R> **flatMap**(Functoin<? super T, ? extends Stream<? extends R>> mapper) | 해당 stream의 요소가 배열일 경우, 배열의 각 요소를 주어진 함수에 인수로 전달하여, 그 반환값으로 이루어진 새로운 stream을 반환함 |
| Stream<T> **distinct**()                                     | 해당 stream에서 중복된 요소가 제거된 새로운 stream을 반환함(내부적으로 Object 클래스의 equals() 메소드를 사용) |
| Stream<T> **limit**(long maxSize)                            | 해당 stream에서 전달된 개수만큼의 요소만으로 이루어진 새로운 stream을 반환함 |
| Stream<T> **peek**(Consumer<? super T> action)               | 결과 stream으로부터 각 요소를 소모하여 추가로 명시된 동작(action)을 수행하여 새로운 stream을 생성하여 반환함 |
| Stream<T> **skip**(long n)                                   | 해당 stream의 첫 번째 요소부터 전달된 개수만큼의 요소를 제외한 나머지 요소만으로 이루어진 새로운 stream을 반환함 |
| Stream<T> **sorted**()<br/>Stream<T> **sorted**(Comparator<? super T> comparator) | 해당 stream을 주어진 비교자(comparator)를 이용하여 정렬, 비교자를 전달하지 않으면 영문사전 순(natural order)으로 정렬함 |

### Stream 필터링

- `filter()` 

  ```java
  final List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
   
  System.out.println(
      list.stream()
          .filter(i -> i<6)
      	.collect(Collectors.toList())
  );
  
  // [1, 2, 3, 4, 5]
  ```

- `distinct()` 메서드는 중복되는 요소를 모두 제거하고 새로운 `stream`을 반환함

  ```java
  List<String> strings = Arrays.asList("google", "apple", "google", "apple", "samsung");
  Stream<String> stream1 = strings.stream();
  
  System.out.println(
      stream1.distinct()
  );
  
  // ["google", "apple", "samsung"]
  ```

### Stream 변환

- `map()`

  ```java
  final List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
   
  System.out.println(
      list.stream()
          .map(i -> i*10)
      	.collect(Collectors.toList())
  );
  
  // [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
  ```

- `flatMap()` 메서드는 평탄화로, 복잡한 자료 구조에 저장된 요소를 단순한 자료 구조로 변환함

  ```java
  String[][] arrays = new String[][]{ {"a1", "a2"}, {"b1", "b2"}, {"c1", "c2", "c3"} };
  Stream<String[]> stream = Arrays.stream(arrays);
  System.out.println(
      stream.flatMap(s -> Arrays.stream(s))
  );
  
  // ["a1", "a2", "b1", "b2", "c1", "c2", "c3"]
  ```

### Stream 제한

- `limit()` 메서드는 첫 번째 요소부터 전달된 갯수만큼의 요소만으로 이루어진 새로운 스트림을 반환함 

  ```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
  Stream<Integer> list = list.stream();
  Stream<Integer> list2 = list.limit(5);

  // [1, 2, 3, 4, 5]
  ```

- `skip()` 메서드는 첫 번째 요소부터 전달된 갯수만큼의 요소를 제외한 나머지 요소로만 이루어진 새로운 스트림을 반환함 

  ```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
  Stream<Integer> list = list.stream();
  Stream<Integer> list2 = list.skip(5);

  // [6, 7, 8, 9, 10]
  ```

### Stream 정렬

- `sorted()` 메서드는 스트림의 요소들이 어떤 기준으로 정렬되어 전달하는 새로운 스트림을 반환함

  ```java
  
  ```

### Stream 연산 결과 확인

- `peek()`

  ```java
  
  ```

## Stream API 최종 연산

`stream` 최종 연산 메서드는 다음과 같이 여러 가지가 있음  

| Method                                                       | Discription                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| void forEach(Consumer<? super T> action)                     | stream의 각 요소에 대해 해당 요소를 소모하여 명시된 동작을 수행함 |
| Optional<T> reduce(BinaryOperator<T> accumulator)<br/>T reduce(T identity, BinaryOperator<T> accumulator) | 처음 두 요소를 가지고 연산을 수행한 뒤, 그 결과와 다음 요소를 가지고 또 다시 연산을 수행함<br />이런 식으로 해당 stream의 모든 요소를 소모하여 연산을 수행하고 그 결과를 반환함 |
| Optional<T> findFirst()<br/>Optional<T> findAny()            | 해당 stream에서 첫 번째 요소를 참조하는 Optional 객체를 반환함(findAny() 메소드는 병렬 stream일 때 사용함) |
| boolean anyMatch(Predicate<? super T> predicate)             | 해당 stream의 일부 요소가 특정 조건을 만족할 경우에 true를 반환함 |
| boolean allMatch(Predicate<? super T> predicate)             | 해당 stream의 모든 요소가 특정 조건을 만족할 경우에 true를 반환함 |
| boolean noneMatch(Predicate<? super T> predicate)            | 해당 stream의 모든 요소가 특정 조건을 만족하지 않을 경우에 true를 반환함 |
| long count()                                                 | 해당 stream의 요소의 개수를 반환함                           |
| Optional<T> max(Comparator<? super T> comparator)            | 해당 stream의 요소 중에서 가장 큰 값을 가지는 요소를 참조하는 Optional 객체를 반환함 |
| Optional<T> min(Comparator<? super T> comparator)            | 해당 stream의 요소 중에서 가장 작은 값을 가지는 요소를 참조하는 Optional 객체를 반환함 |
| T sum()                                                      | 해당 stream의 모든 요소에 대해 합을 구하여 반환함            |
| Optional<T> average()                                        | 해당 stream의 모든 요소에 대해 평균값을 구하여 반환함        |
| <R,A> R collect(Collector<? super T,A,R> collector)          | 인수로 전달되는 Collectors 객체에 구현된 방법대로 stream의 요소를 수집함 |

### Stream 요소 출력

`forEach()`

```java

```

### Stream 요소 소모

`reduce()`

```java
```

### Stream 요소 검색

- `findFirst()`

  ```java
  ```

- `findAny()`

  ```java
  
  ```

### Stream 요소 검사

- `anyMatch()`

  ```java
  ```

- `allMatch()`

  ```java
  ```

- `noneMatch()`

  ```java
  
  ```

### Stream 요소 통계

- `count()`

  ```java
  ```

- `min()`

  ```java
  ```

- `max()`

  ```java
  
  ```

### Stream 요소 연산

- `sum()`

  ```java
  ```

- `average()`

  ```java
  
  ```

### Stream 요소 수집

`stream` 요소의 수집 용도별로 `collect()`

```java
```



## Reference

- https://thalals.tistory.com/321
- https://codechacha.com/ko/stream-filter/
- https://codechacha.com/ko/java8-stream-distinct/
- https://codechacha.com/ko/java8-stream-limit-skip/
- https://codechacha.com/ko/java8-stream-sorted/
