# Lamda, Functional Interface

# 람다식(Lambda Expression) 이란?

- 람다식은 함수형 프로그래밍을 위해 등장한 개념으로, 메서드를 하나의 식으로 표현한 것이다.

- 람다식은 익명 함수를 생성하기 위한 식으로, 함수형 인터페이스의 익명 구현 객체를 생성하기 위해 사용된다.

- 람다식은 메서드를 간결하게 표현할 수 있으며, 코드를 줄일 수 있다.

- 람다식은 메서드를 매개변수로 전달하거나 메서드의 결과값으로 반환할 수 있다.

- 람다식은 함수형 인터페이스를 구현할 때 사용된다.

### 람다식의 문법 예시

```java
(매개변수, ...) -> { 실행문; ... }
```

- 매개변수가 하나인 경우, 괄호를 생략할 수 있다.

```java
매개변수 -> { 실행문; ... }
```

- 실행문이 하나인 경우, 중괄호를 생략할 수 있다.

```java
매개변수 -> 실행문;
```

### 기존 함수 표현 vs 람다식 표현

```java
// 기존 함수 표현
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(i);
        }
    }
};

// 람다식 표현
Runnable runnable = () -> {
    for (int i = 0; i < 10; i++) {
        System.out.println(i);
    }
};
```

```java
// 기존 함수 표현
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(i);
        }
    }
});

// 람다식 표현
Thread thread = new Thread(() -> {
    for (int i = 0; i < 10; i++) {
        System.out.println(i);
    }
});
```

```java
// 기존 함수 표현
List<String> list = new ArrayList<>();

list.add("apple");
list.add("banana");

list.sort(new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        return o1.compareTo(o2);
    }
});

// 람다식 표현
List<String> list = new ArrayList<>();

list.add("apple");
list.add("banana");

list.sort((o1, o2) -> o1.compareTo(o2));
```



## 람다식의 특징

1. 람다식 내에서 사용되는 지역변수는 final이 붙지 않아도 상수로 간주된다.
2. 람다식으로 선언된 변수명은 다른 변수명과 중복될 수 없다.

## 람다식의 장점

1. 코드를 간결하게 만들 수 있다.
2. 식에 개발자의 의도가 명확히 드러나 가독성이 높아진다.
3. 함수를 만드는 과정없이 한번에 처리할 수 있어 생산성이 높아진다.
4. 병렬프로그래밍이 용이하다.

## 람다식의 단점

1. 람다를 사용하면서 만든 익명함수는 재사용이 불가능하다.
2. 디버깅이 어렵다.
3. 람다를 남발하면 비슷한 함수가 중복 생성되어 코드가 지저분해질 수 있다.
4. 재귀로 만들경우 부적합하다.

# 함수형 인터페이스(Functional Interface)

# 함수형 인터페이스(Functional Interface)란?

- 함수형 인터페이스는 하나의 추상 메서드를 가지고 있는 인터페이스이다.

- 함수형 인터페이스는 람다식을 사용하기 위해 사용된다.

- 함수형 인터페이스는 `@FunctionalInterface` 어노테이션을 사용하여 선언한다.

- 함수형 인터페이스는 자바에서 제공하는 함수형 인터페이스를 사용할 수도 있고, 직접 함수형 인터페이스를 선언하여 사용할 수도 있다.

### 예시

```java

@FunctionalInterface
public interface MyFunctionalInterface {
    public void method();
}

```

### 자바에서 제공하는 함수형 인터페이스

- `java.util.function` 패키지에는 자바에서 제공하는 함수형 인터페이스가 정의되어 있다.

- 자바에서 제공하는 함수형 인터페이스는 다양한 용도로 사용할 수 있다.

- 자바에서 제공하는 함수형 인터페이스는 다음과 같다.

| 함수형 인터페이스 | 설명 |
|---|---|
| `Supplier<T>` | 매개변수 없이 리턴값만 있는 경우 사용 |
| `Consumer<T>` | 매개변수만 있고 리턴값이 없는 경우 사용 |
| `Function<T, R>` | 매개변수와 리턴값이 모두 있는 경우 사용 |
| `Predicate<T>` | 매개변수를 받아서 boolean 값을 리턴하는 경우 사용 |
| `UnaryOperator<T>` | 매개변수와 리턴값이 같은 경우 사용 |
| `BinaryOperator<T>` | 매개변수가 두개이고 리턴값이 하나인 경우 사용 |


## Supplier<T>

- 매개변수 없이 리턴값만 있는 경우 사용한다.

- `T get()` 메서드를 제공한다.

```java
Supplier<String> supplier = () -> "Hello World";

String result = supplier.get();

System.out.println(result); // Hello World
```

## Consumer<T>

- 매개변수만 있고 리턴값이 없는 경우 사용한다.

- `void accept(T t)` 메서드를 제공한다.

```java
Consumer<String> consumer = (str) -> System.out.println(str);

consumer.accept("Hello World"); // Hello World
```

## Function<T, R>

- 매개변수와 리턴값이 모두 있는 경우 사용한다.

- `R apply(T t)` 메서드를 제공한다.

```java
Function<String, Integer> function = (str) -> str.length();

int result = function.apply("Hello World");

System.out.println(result); // 11
```

## Predicate<T>

- 매개변수를 받아서 boolean 값을 리턴하는 경우 사용한다.

- `boolean test(T t)` 메서드를 제공한다.

```java
Predicate<Integer> predicate = (num) -> num > 10;

boolean result = predicate.test(5);

System.out.println(result); // false
```

## UnaryOperator<T>

- 매개변수와 리턴값이 같은 경우 사용한다.

- `T apply(T t)` 메서드를 제공한다.

```java
UnaryOperator<Integer> unaryOperator = (num) -> num * 10;

int result = unaryOperator.apply(5);

System.out.println(result); // 50
```

## BinaryOperator<T>

- 매개변수가 두개이고 리턴값이 하나인 경우 사용한다.

- `T apply(T t1, T t2)` 메서드를 제공한다.

```java
BinaryOperator<Integer> binaryOperator = (num1, num2) -> num1 + num2;

int result = binaryOperator.apply(5, 10);

System.out.println(result); // 15
```

## 메소드 참조 (Method Reference)

- 람다식을 더 간결하게 표현하는 방법으로, 람다식에서 불필요한 매개변수를 제거하고, 메서드의 이름을 직접 참조하는 방법이다.

- 메소드 참조는 `::` 기호를 사용하여 표현한다.

- 메소드 참조는 다음과 같은 방법으로 사용할 수 있다.

```java
// 람다식
Function<String, Integer> function = (str) -> str.length();

// 메소드 참조
Function<String, Integer> function = String::length;
```

### 메소드 참조의 종류

1. `static` 메서드 참조

```java
Function<String, Integer> function = String::length;
```

2. `instance` 메서드 참조

```java
String str = "Hello World";

Function<Integer, Character> function = str::charAt;
```

3. 특정 객체의 인스턴스 메서드 참조

```java
String str = "Hello World";

Function<Integer, Character> function = str::charAt;
```

4. 생성자 참조

```java
Supplier<String> supplier = String::new;
```

<br>

# 함수형 인터페이스의 활용

## 함수형 인터페이스를 매개변수로 사용하기

- 함수형 인터페이스를 매개변수로 사용하여 메서드를 호출할 수 있다.

```java
public class Main {

    public static void main(String[] args) {
        execute(() -> System.out.println("Hello World"));
    }

    public static void execute(MyFunctionalInterface myFunctionalInterface) {
        myFunctionalInterface.method();
    }

}
```

## 함수형 인터페이스를 리턴값으로 사용하기

- 함수형 인터페이스를 리턴값으로 사용하여 메서드를 호출할 수 있다.

```java
public class Main {

    public static void main(String[] args) {
        MyFunctionalInterface myFunctionalInterface = getMyFunctionalInterface();
        myFunctionalInterface.method();
    }

    public static MyFunctionalInterface getMyFunctionalInterface() {
        return () -> System.out.println("Hello World");
    }

}
```

## 함수형 인터페이스를 필드로 사용하기

- 함수형 인터페이스를 필드로 사용하여 메서드를 호출할 수 있다.

```java
public class Main {

    private static MyFunctionalInterface myFunctionalInterface;

    public static void main(String[] args) {
        myFunctionalInterface = () -> System.out.println("Hello World");
        myFunctionalInterface.method();
    }

}
```

## 함수형 인터페이스를 배열로 사용하기

- 함수형 인터페이스를 배열로 사용하여 메서드를 호출할 수 있다.

```java
public class Main {

    public static void main(String[] args) {
        MyFunctionalInterface[] myFunctionalInterfaces = new MyFunctionalInterface[5];

        for (int i = 0; i < myFunctionalInterfaces.length; i++) {
            myFunctionalInterfaces[i] = () -> System.out.println("Hello World");
        }

        for (MyFunctionalInterface myFunctionalInterface : myFunctionalInterfaces) {
            myFunctionalInterface.method();
        }
    }

}
```

## 함수형 인터페이스를 컬렉션으로 사용하기

- 함수형 인터페이스를 컬렉션으로 사용하여 메서드를 호출할 수 있다.

```java
public class Main {

    public static void main(String[] args) {
        List<MyFunctionalInterface> myFunctionalInterfaces = new ArrayList<>();

        for (int i = 0; i < 5; i++) {
            myFunctionalInterfaces.add(() -> System.out.println("Hello World"));
        }

        for (MyFunctionalInterface myFunctionalInterface : myFunctionalInterfaces) {
            myFunctionalInterface.method();
        }
    }

}
```

# 함수형 인터페이스의 활용 예시

## Runnable 인터페이스

- `Runnable` 인터페이스는 함수형 인터페이스로, 스레드를 생성할 때 사용된다.

- `Runnable` 인터페이스는 `void run()` 메서드를 제공한다.

```java

public class Main {

    public static void main(String[] args) {
        Runnable runnable = () -> {
            for (int i = 0; i < 10; i++) {
                System.out.println(i);
            }
        };

        Thread thread = new Thread(runnable);
        thread.start();
    }

}

```

## Comparator 인터페이스

- `Comparator` 인터페이스는 함수형 인터페이스로, 정렬을 할 때 사용된다.

- `Comparator` 인터페이스는 `int compare(T o1, T o2)` 메서드를 제공한다.

```java
public class Main {

    public static void main(String[] args) {
        List<String> list = new ArrayList<>();

        list.add("apple");
        list.add("banana");
        list.add("orange");

        list.sort((o1, o2) -> o1.compareTo(o2));

        for (String str : list) {
            System.out.println(str);
        }
    }

}
```

## ActionListener 인터페이스

- `ActionListener` 인터페이스는 함수형 인터페이스로, 이벤트를 처리할 때 사용된다.

- `ActionListener` 인터페이스는 `void actionPerformed(ActionEvent e)` 메서드를 제공한다.

```java
public class Main {

    public static void main(String[] args) {
        JButton button = new JButton("Click");

        button.addActionListener(e -> System.out.println("Hello World"));
    }

}
```

## Callable 인터페이스

- `Callable` 인터페이스는 함수형 인터페이스로, 스레드를 생성할 때 사용된다.

- `Callable` 인터페이스는 `V call()` 메서드를 제공한다.

```java
public class Main {

    public static void main(String[] args) {
        Callable<String> callable = () -> "Hello World";

        ExecutorService executorService = Executors.newFixedThreadPool(1);
        Future<String> future = executorService.submit(callable);

        try {
            String result = future.get();
            System.out.println(result);
        } catch (Exception e) {
            e.printStackTrace();
        }

        executorService.shutdown();
    }

}
```
