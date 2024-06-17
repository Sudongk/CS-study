# Stream API

## Stream API란?

- 자바 8부터 추가된 Stream API는 컬렉션, 배열 등의 데이터를 함수형 프로그래밍 방식으로 처리할 수 있도록 도와주는 API이다. Stream API를 사용하면 데이터를 필터링하거나 매핑, 정렬 등 다양한 연산을 수행할 수 있다.

- Stream API는 데이터 소스로부터 데이터를 읽기만 할 뿐, 데이터를 변경하지 않는다. 따라서 Stream API를 사용하면 데이터 소스의 원본 데이터를 변경하지 않고 원하는 결과를 얻을 수 있다.

- 개발자가 간결하고 읽을 수 있는 코드로 컬렉션에 대한 복잡한 작업을 수행할 수 있다.

- 스트림은 병렬처리가 가능하도록 설계되었으므로 멀티 코어 프로세서를 활용하여 처리 속도를 높일 수 있다.

## Stream API의 주요 기능

- **Functional programming:** 스트림은 함수형 프로그래밍 기술과 함께 사용되도록 설계 되어서 람다 식과 참조를 사용하여 집합에 대한 작업을 수행할 수 있음.
- **Lazy evaluation:** 필요할 때만 스트림의 요소를 평가하여 메모리 사용량을 줄이고 성능을 향상시킬 수 있음.
- **Parallel processing:** 병렬 처리가 가능하도록 설계되었으므로 멀티 코어 프로세서를 활용하여 처리 속도를 높일 수 있음.
- **Intermediate and terminal operations:** 중간 및 종료의 두 가지 유형의 작업을 지원함. 필터 및 맵과 같은 중간 작업은 추가로 처리할 수 있는 새 스트림을 반환함. 종료 작업은 최종 결과를 생성.

## Stream API의 몇 가지 추가 기능:

- **Reduction operations:** 데이터를 단일 값으로 줄이는 데 도움아 되는 ‘average()’, ‘sum()’, ‘count()’와 같은 연산을 사용하여 데이터 수집에 대한 통계를 계산할 수 있음.
- **Short-circuiting operations:** findFirst(), findAny()와 연산을 사용하여 지정된 술어와 일치하는 스트림의 첫 번째 또는 임의 요소를 찾을 수 있음. 이러한 작업은 특정 시나리오에서 성능을 향상시키는데 도움이 될 수 있음.
- **Collectors:** collecotrs는 스트림의 요소를 집합 또는 다른 데이터 구조로 축적하는 데 사용됨. java는 스트림의 요소를 수집하는데 사용할 수 있는 ‘toList()’, ‘toSet()’과 같은 가양한 기본 제공 collectors를 제공.

## Stream API의 특징

- 원본의 데이터를 변경하지 않는다.
원본의 데이터를 조회하여 별도의 요소들로 stream을 생성한다. 원본의 데이터를 읽기만 하고, 정렬이나 필터링 등의 작업은 별도의 stream 요소들에서 처리가 됨.
- 일회용이다.
한번 사용하면 재사용이 불가능하다. stream이 또 필요할 경우 다시 생성해주어야 함.
- 내부 반복으로 작업을 처리한다.
기존에는 반복문을 사용하기 위해 for이나 while과 같은 문법을 사용해야 했지만, stream에서는 그런 문법을 메소드 내에 숨기고 있기 때문에, 보다 간결한 코드의 작성이 가능.
- 스트림의 연산은 필터(filter)-맵(map)기반의 API를 사요앟여 지연 연산을 통해 성능을 최적화 한다.
- parallelStream() 메서드를 통한 손쉬운 병렬 처리를 지원한다.

## Stream API의 종류

- **BaseStream:** 모든 스트림의 최상위 인터페이스로, 기본적인 스트림의 기능을 제공한다.

- **Stream:** 객체 요소를 처리하는데 사용되는 스트림이다.

- **IntStream:** int 요소를 처리하는데 사용되는 스트림이다.

- **LongStream:** long 요소를 처리하는데 사용되는 스트림이다.

- **DoubleStream:** double 요소를 처리하는데 사용되는 스트림이다.

## Stream API의 사용

- Stream API를 사용하려면 java.util.stream 패키지를 import 해야 한다.

```java
import java.util.stream.Stream;

public class StreamExample {
    public static void main(String[] args) {
        Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
        stream.forEach(System.out::println);
    }
}
```

### Stream의 생성
스트림 API는 다양항 데이터 소스에서 생성할 수 있음.

1. 컬렉션

자바에서 제공하는 모든 컬렉션의 최고 상위 조상인 Collection 인터페이스에 stream()메소드가 정의되어 있음.

```java
List<String> list = Arrays.asList("Java", "Python", "C++", "JavaScript");
Stream<String> stream = list.stream();
```

2. 배열

배열에 관한 스트림을 생성하기 위해 Arrays 클래스에는 다양한 형태의 stream() 메소드가 정의되어 있음.
기본 타입을 저장할 수 있는 배열에 관한 스트림이 별도로 정의되어 있음.
이러한 스트림은 java.util.stream 패키지의 IntStream, LongStream, DoubleStream 인터페이스로 각각 제공 됨.

```java
String[] arr = {"Java", "Python", "C++", "JavaScript"};
Stream<String> stream = Arrays.stream(arr);
```

3. 가변 매개변수

Stream 클래스의 of() 메소드를 사용하면 가변 매개변수를 전달받아 스트림을 생성할 수 있음.

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
```

4. 지정된 범위의 연속된 정수

IntStream이나 LongStream 인터페이스에는 range()와 rangeClosed() 메소드가 정의되어 있음.

range()메소드는 시작 정수를 포함하지만 마지막 정수는 포함하지 않음.
rangeClosed()메소드는 시작 정수와 마지막 정수까지도 포함하는 스트림 생성.

```java
IntStream stream = IntStream.range(1, 5);
stream.forEach(System.out::println);
```

```java
IntStream stream = IntStream.rangeClosed(1, 5);
stream.forEach(System.out::println);
```

5. 특정 타입의 난수들

특정 타입의 난수로 이루어진 스트림을 생성하기 위해 Random 클래스에는 ints(), longs(), doubles()와 같은 메소드가 정의되어 있음. 매개변수로 long타입으로 전달받을 수 있음. 매개변수를 전달하지 않으면 크기가 정해지지 않은 무한 스트림을 반환하므로 limit() 메소드를 사용하여 따로 스트림의 크기를 제한해야함.

```java
IntStream stream = new Random().ints(4, 1, 100);
stream.forEach(System.out::println);
```


6. 람다 표현식

람다 표현식을 매개변수로 전달받아 해당 람다 표현식에 의해 반환되는 값을 요소로 하는 무한 스트림을 생성하기 위해 stream 클래스에는 iterate()와 generate() 메서드가 정의되어 있음.

```java
IntStream stream = IntStream.iterate(2, n -> n + 2).limit(10);
stream.forEach(System.out::println);
```

7. 파일

파일의 한행을 요소로 하는 스트림을 생성하기 위해 java.nio.file.Files 클래스에는 lines() 메소드가 정의되어 있음.
java.io.BufferedReader 클래스의 lines() 메소드를 사용하면 파일뿐만 아니라 다른 입력으로부터도 데이터를 행 당위로 읽어 올 수 있음.

```java
try (Stream<String> stream = Files.lines(Paths.get("file.txt"))) {
    stream.forEach(System.out::println);
} catch (IOException e) {
    e.printStackTrace();
}
```

8. 빈 스트림

아무 요소도 가지지 않는 빈 스트림은 Stream 클래스의 empty() 메소드를 사용하여 생성 가능

```java
Stream<Object> stream = Stream.empty();
stream.forEach(System.out::println);
```

### 스트림의 중개 연산

초기 스트림은 중개 연산을 통해 또 다른 스트림으로 변환되고 이러한 중개 연산은 스트림을 전달받아 스트림을 반환하므로, 중개 연산은 연속으로 연결해서 사용할 수 있음.

1. 스트림 필터링
 - filter() : 해당 스트림에서 주어진 조건에 맞는 요소만으로 구성된 새로운 스트림 반환
 - distinct() : 내부적으로 Object 클래스의 equals() 메소드를 사용하여 요소의 중복 비교하고 중복 요소 제거

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.filter(s -> s.length() > 4).forEach(System.out::println);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.distinct().forEach(System.out::println);
```

2. 스트림 변환
 - map() : 해당 스트림의 요소들을 주어진 함수에 인수로 전달하여, 그 반환값들로 이루어진 새로운 스트림 반환
 - flatMap() : 각 배열의 각 요소의 반환값을 하나로 합친 새로운 스트림을 얻을 수 있음.

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.map(String::toUpperCase).forEach(System.out::println);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.flatMap(s -> Stream.of(s.split(""))).forEach(System.out::println);
```

3. 스트림 제한
 - limit() : 첫 번째 요소부터 전달된 개수만큼의 요소만으로 이루어진 새로운 스트림 반환
 - skip() : 첫 번째 요소부터 잔달된 개수만큼의 요소를 제외한 나머지 요소만으로 이루어진 새로운 스트림 반환

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.limit(2).forEach(System.out::println);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.skip(2).forEach(System.out::println);
```

4. 스트림 정렬
 - sorted() : 주어진 비교자를 이용하여 정렬하고 비교자를 전달하지 않으면 기본적으로 오름차순 정렬
    - sorted(Comparator.reverseOrder()) : 내림차순 정렬

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.sorted().forEach(System.out::println);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.sorted(Comparator.reverseOrder()).forEach(System.out::println);
```

5. 스트림 연산 결과 확인
 - peek() : 원본 스트림에서 요소를 소모하지 않고, 연산과 연산 사이에 결과를 확인하고 싶을 때 사용.

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.peek(System.out::println).forEach(System.out::println);
```

### 스트림의 최종 연산

중개 연산을 통해 변환된 스트림은 마지막으로 최종 연산을 통해 각 요소를 소모하여 결과 표시.
지연 되었던 모든 중개 연산들이 최종 연산에서 모두 수행되고 해당 스트림은 더 이상 사용할 수 없음.

1. 요소의 출력
 - forEach() : 스트림의 각 요소를 소모하여 명시된 동작 수행. 반환 타입이 void 이므로 스트림의 모든 요소를 출력하는 용도로 많이 사용

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.forEach(System.out::println);
```

2. 요소의 소모
 - reduce() : 첫 번째와 두 번째 요소를 가지고 연산을 수행한 뒤, 그 결과와 세변째 요소를 가지고 또다시 연산 수행. 
인수로 초기값을 전달하면 초기값과 해당 스트림의 첫 번째 요소로 연산을 시작하며, 그 결과와 두 번째 요소를 가지고 걔속해서 연산 수행.

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
String result = stream.reduce("", (a, b) -> a + b);
System.out.println(result);
```

3. 요소의 검색
 - findFirst(), findAny() : 첫 번째 요소를 참조하는 Optional 객체 반환. 병렬 스트림일 경우 findAny() 메소드를 사용해야만 정확한 연산 결과 반환.

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
Optional<String> result = stream.findFirst();
System.out.println(result.get());
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
Optional<String> result = stream.findAny();
System.out.println(result.get());
```

4. 요소의 검사
 - anyMatch() : 해당 스트림의 일부 요소가 특정 조건을 만족할 경우 true 반환
 - allMatch() : 해당 스트림의 모든 요소가 특정 조건을 만족할 경우 true 반환
 - noneMatch() : 해당 스트림의 모든 요소가 특정 조건을 만족하지 않을 경우 true 반환

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
boolean result = stream.anyMatch(s -> s.contains("Java"));
System.out.println(result);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
boolean result = stream.allMatch(s -> s.contains("Java"));
System.out.println(result);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
boolean result = stream.noneMatch(s -> s.contains("Java"));
System.out.println(result);
```

5. 요소의 통계
 - count() : 해당 스트림의 요소의 총 개수를 long타입으로 반환
 - max(), min() : 해당 스트림의 요소 중 가장 큰 값과 가장 작은 값을 참조하는 Optional 객체 반환

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
long result = stream.count();
System.out.println(result);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
Optional<String> result = stream.max(Comparator.naturalOrder());
System.out.println(result.get());
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
Optional<String> result = stream.min(Comparator.naturalOrder());
System.out.println(result.get());
```

6. 요소의 연산
 - sum(), average() : IntStream, DoubleStream과 같은 기본 타입 스트림에는 모든 요소에 대해 합과 평균을 구할 수 있는 메소드가 정의되어 있음. average() 메소드는 각 기본 타입으로 래핑 된 Optional 객체 반환

```java
IntStream stream = IntStream.of(1, 2, 3, 4, 5);
int result = stream.sum();
System.out.println(result);
```

```java
IntStream stream = IntStream.of(1, 2, 3, 4, 5);
OptionalDouble result = stream.average();
System.out.println(result.getAsDouble());
```

7. 요소의 수집

collect() 메소드는 인수로 전달되는 Collectors 객체에 구현된 방법대로 스트림의 요소를 수집

 - 스트림을 배열이나 컬렉션으로 변환 : toArray(), toCollection(), toList(), toSet(), toMap()
 - 요소의 통계와 연산 메소드와 같은 동작 수행 : counting(), maxBy(), minBy(), summingInt(), averagingInt() 등
 - 요소의 소모와 같은 동작을 수행 : reducing(), joining()
 - 요소의 그룹화와 분할 : groupingBy(), partitioningBy()

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
List<String> result = stream.collect(Collectors.toList());
System.out.println(result);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
String result = stream.collect(Collectors.joining(", "));
System.out.println(result);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
Map<String, Integer> result = stream.collect(Collectors.toMap(Function.identity(), String::length));
System.out.println(result);
```

### 병렬 스트림

스트림 API는 병렬 처리를 위한 parallelStream() 메소드를 제공하며, 이 메소드를 사용하면 멀티 코어 프로세서를 활용하여 처리 속도를 높일 수 있음.

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.parallel().forEach(System.out::println);
```

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.parallel().forEachOrdered(System.out::println);
```

### Stream API의 예외 처리

스트림 API는 예외 처리를 위한 try-catch 블록을 사용할 수 있음.

```java
Stream<String> stream = Stream.of("Java", "Python", "C++", "JavaScript");
stream.forEach(s -> {
    try {
        System.out.println(s.length());
    } catch (Exception e) {
        System.out.println("Exception: " + e.getMessage());
    }
});
```

### Stream API의 활용

- **데이터 필터링:** 스트림을 사용하여 데이터를 필터링할 수 있음.
- **데이터 매핑:** 스트림을 사용하여 데이터를 매핑할 수 있음.
- **데이터 정렬:** 스트림을 사용하여 데이터를 정렬할 수 있음.
- **데이터 그룹화:** 스트림을 사용하여 데이터를 그룹화할 수 있음.
- **데이터 집계:** 스트림을 사용하여 데이터를 집계할 수 있음.

## Stream API의 예제

```java
import java.util.Arrays;

public class StreamExample {
    public static void main(String[] args) {
        // Stream 생성
        String[] arr = {"Java", "Python", "C++", "JavaScript"};
        Arrays.stream(arr).forEach(System.out::println);

        // Stream 필터링
        Arrays.stream(arr).filter(s -> s.length() > 4).forEach(System.out::println);

        // Stream 매핑
        Arrays.stream(arr).map(String::toUpperCase).forEach(System.out::println);

        // Stream 정렬
        Arrays.stream(arr).sorted().forEach(System.out::println);

        // Stream 연산 결과 확인
        Arrays.stream(arr).peek(System.out::println).forEach(System.out::println);

        // Stream 최종 연산
        Arrays.stream(arr).forEach(System.out::println);
        String result = Arrays.stream(arr).reduce("", (a, b) -> a + b);
        System.out.println(result);
        System.out.println(Arrays.stream(arr).findFirst().get());
        System.out.println(Arrays.stream(arr).anyMatch(s -> s.contains("Java")));
        System.out.println(Arrays.stream(arr).count());
    }
}
```

## Stream API의 주의사항

- **스트림은 일회용이다.** 한 번 사용하면 재사용이 불가능하다. 스트림이 또 필요할 경우 다시 생성해주어야 한다.

- **스트림은 내부 반복으로 작업을 처리한다.** 기존에는 반복문을 사용하기 위해 for이나 while과 같은 문법을 사용해야 했지만, 스트림에서는 그런 문법을 메소드 내에 숨기고 있기 때문에, 보다 간결한 코드의 작성이 가능하다.

- **스트림의 연산은 필터(filter)-맵(map)기반의 API를 사용하여 지연 연산을 통해 성능을 최적화한다.**

- **스트림은 병렬 처리가 가능하도록 설계되었으므로 멀티 코어 프로세서를 활용하여 처리 속도를 높일 수 있다.**

- **스트림은 원본의 데이터를 변경하지 않는다.** 원본의 데이터를 조회하여 별도의 요소들로 stream을 생성한다. 원본의 데이터를 읽기만 하고, 정렬이나 필터링 등의 작업은 별도의 stream 요소들에서 처리가 된다.

## Stream API의 장단점

### 장점

- **간결한 코드 작성:** 스트림 API를 사용하면 코드를 간결하게 작성할 수 있다. 스트림 API를 사용하면 반복문을 사용하지 않고도 데이터를 처리할 수 있다.

- **성능 향상:** 스트림 API는 내부적으로 병렬 처리를 지원하므로 멀티 코어 프로세서를 활용하여 처리 속도를 높일 수 있다.

- **함수형 프로그래밍:** 스트림 API는 함수형 프로그래밍 기술과 함께 사용되도록 설계 되어서 람다 식과 참조를 사용하여 집합에 대한 작업을 수행할 수 있다.

### 단점

- **학습 곡선:** 스트림 API는 처음에는 사용하기 어려울 수 있으며, 학습 곡선이 높을 수 있다.

- **디버깅:** 스트림 API를 사용하면 코드가 간결해지지만, 디버깅이 어려울 수 있다.

- **성능:** 스트림 API를 사용하면 코드가 간결해지지만, 성능이 떨어질 수 있다.
