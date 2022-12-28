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
- 기본적으로 String에 대한 Comparable이 구현되어있으며, 다른 정렬기준이 필요하다면 인자로 전달
- 역순으로 정렬하고 싶다면 `.reversed()`를 붙임

  ```java
  List<String> langs = Arrays.asList("java", "kotlin", "haskell", "ruby", "smalltalk");
  langs.stream()
       .sorted()
       .forEach(System.out::println);
  // haskell
  // java
  // kotlin
  // ruby
  // smalltalk
  ```
  ```java
  List<String> langs = Arrays.asList("java", "kotlin", "haskell", "ruby", "smalltalk");
  langs.stream()
       .sorted(Comparator.comparing(String::length))
       .forEach(System.out::println);
  // java
  // ruby
  // kotlin
  // haskell
  // smalltalk
  ```

### Stream 연산 결과 확인

- `peek()` 메서드는 전체 요소를 반복하는 루핑에 해당하는 메서드로 이를 통해 연산결과 등을 확인함  
- `forEach()` 메서드와 동일하게 stream의 요소를 소모하는 연산이지만, 중간 연산이므로 최종 연산 메서드가 실행되지 않으면 동작하지 않음  

  ```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
  Stream<Integer> list = list.stream()
                             .peek(System.out::println)
                             .sum(); // 최종연산이 있어 peek도 정상 실행
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

`forEach()` 메서드는 List, Map 등을 순회하는데 사용하는 최종연산임  
단, 반복문에서 특정 조건일 경우 순회를 중단할 수 없으므로 비효율적인 부분이 있음  
또한 동시성이 보장되지 않으므로 일반적으로 명시된 동작에서 상태를 수정한다면 오류가 발생할 수 있음  
따라서 객체의 데이터를 다룰 때 `forEach()`를 사용한다면 주의할 필요가 있음  

```java
  List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
  list.forEach(System.out::println);
```

### Stream 요소 소모

`reduce()` 메서드는 stream의 요소들을 하나의 데이터로 만드는 작업을 수행함  
연산하는 부분은 직접 구현해 인자로 전달해야 하며, 초기값도 인자로 전달할 수 있음  

```java
  Stream<Integer> numbers = Stream.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
  Optional<Integer> sum = numbers.reduce(10, (x, y) -> x + y);
  sum.ifPresent(s -> System.out.println("sum: " + s));
  // sum: 65
```

### Stream 요소 검색

- `findFirst()` 메서드는 조건에 일치하는 요소들 중 순서가 가장 앞에 있는 요소 1개를 optional로 리턴하며, 일치하는 요소가 없다면 empty가 리턴됨  

```java
  List<String> elements = Arrays.asList("a", "a1", "b", "b1", "c", "c1");
  Optional<String> firstElement = elements.stream()
                                          .filter(s -> s.startsWith("b")).findFirst();
  System.out.println("findFirst: " + firstElement.get());
  // findFirst: b
```

- `findAny()` 메서드는 가장 먼저 탐색되는 요소를 리턴  

```java
  List<String> elements = Arrays.asList("a", "a1", "b", "b1", "c", "c1");
  Optional<String> firstElement = elements.stream()
                                          .filter(s -> s.startsWith("b")).findAny();
  System.out.println("findFirst: " + firstElement.get());
  // findAny: b
```

### Stream 요소 검사

- `anyMatch()` 메서드는 Stream에 조건에 부합하는 객체가 1개라도 있는지 탐색하고 boolean 타입으로 결괴를 리턴함  

  ```java
    List<String> elements = Arrays.asList("a", "a1", "b", "b1", "c", "c1");

    boolean anyMatch = elements.stream().anyMatch(s -> s.startsWith("b"));
    System.out.println("anyMatch: " + (anyMatch ? "true" : "false"));
    // anyMatch: true
  ```

- `allMatch()` 메서드는 모든 객체가 조건에 부합하는지 탐색하고 boolean 타입으로 결괴를 리턴함  

  ```java
    List<String> elements = Arrays.asList("a", "a1", "b", "b1", "c", "c1");

    boolean anyMatch = elements.stream().allMatch(s -> s.startsWith("b"));
    System.out.println("allMatch: " + (allMatch ? "true" : "false"));
    // allMatch: false
  ```

- `noneMatch()` 메서드는 조건에 부합하는 객체가 없는지 탐색하고 boolean 타입으로 결과를 리턴함  

  ```java
    List<String> elements = Arrays.asList("a", "a1", "b", "b1", "c", "c1");

    boolean anyMatch = elements.stream().noneMatch(s -> s.startsWith("b"));
    System.out.println("noneMatch: " + (noneMatch ? "true" : "false"));
    // noneMatch: false
  ```

### Stream 요소 통계

- `count()` 메서드는 stream의 요소들을 처리한 뒤 하나의 값으로 산출하는 리덕션이며, 스트림 요소의 총 개수를 리턴  

  ```java
    List<String> words = Arrays.asList("book", "desk", "keyboard", "mouse", "cup");
   	int count = (int) words.stream().filter(w->w.contains("o")).count();
   	System.out.println("count >" + count);
    // count > 3
  ```

- `min()` 메서드도 리덕션이며, 스트림 요소 중 최소값을 리턴  

  ```java
    List<Integer> cal = Arrays.asList(49, 123, 22, 55, 21);
    int min = cal.stream().min(Comparator.comparing(x-> x)).get();
    System.out.println("min >" + min);
    // min > 21
  ```

- `max()` 메서드도 리덕션이며, 스트림 요소 중 최값을 리턴  

  ```java
    List<Integer> cal = Arrays.asList(49, 123, 22, 55, 21);
    int max = cal.stream().max(Comparator.comparing(x-> x)).get();
    System.out.println("max >" + max);
    // max > 123
  ```

### Stream 요소 연산

- `sum()`메서드도 리덕션이며, 스트림 요소의 총합을 리턴  

  ```java
    List<Integer> cal = Arrays.asList(1, 2, 3, 4, 5);
    long sum = Arrays.stream(cal).sum();
    System.out.println("sum >" + sum);
    // sum > 15
  ```

- `average()`메서드도 리덕션이며, 스트림 요소 의 평균값을 리턴  

  ```java
    List<Integer> cal = Arrays.asList(1, 2, 3, 4, 5);
    double avg = Arrays.stream(cal).average().getAsDouble();
    System.out.println("avg >" +avg);
    // avg > 3
  ```

### Stream 요소 수집

`collect()` 메서드는 Stream 데이터를 변형 등의 처리를 하고 원하는 자료형으로 반환함  
아래와 같은 기능 등을 제공함  
- Stream의 아이템들을 List 또는 Set 자료형으로 반환
- Stream의 아이템들을 joining
- Stream의 아이템들을 sorting하여 가장 큰 객체 리턴
- Stream의 아이템들의 평균값을 리턴


```java
  Stream<String> fruits = Stream.of("banana", "apple", "mango", "kiwi", "peach", "cherry", "lemon");
  HashSet<String> fruitHashSet = fruits.collect(HashSet::new, HashSet::add, HashSet::addAll);
```
위의 예제와 같이 `supplier`, `accumulator`, `combiner` 3개의 인자를 받아 Stream의 요소들을 다른 데이터로 변환할 수 있으나  
`Collectors`라는 라이브러리를 통해 3개의 인자를 넣지 않고도 기본적인 기능을 제공받을 수 있음  

```java
  Stream<String> fruits = Stream.of("banana", "apple", "mango", "kiwi", "peach", "cherry", "lemon");
  Set<String> fruitSet = fruits.collect(Collectors.toSet());
```



## Reference

- https://thalals.tistory.com/321
- https://codechacha.com/ko/stream-filter/
- https://codechacha.com/ko/java8-stream-distinct/
- https://codechacha.com/ko/java8-stream-limit-skip/
- https://codechacha.com/ko/java8-stream-sorted/
- https://codechacha.com/ko/java8-stream-reduction/
- https://codechacha.com/ko/java8-stream-difference-findany-findfirst/
- https://codechacha.com/ko/java8-stream-collect/
- https://codechacha.com/ko/java8-summarystatistics/
- https://cornswrold.tistory.com/299
