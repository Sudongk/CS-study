## Java 17 특징

### Java11 이후의 두번쨰 LTS(Long-Term Support) 버전

- Java 17은 Java 11 이후의 두번째 LTS(Long-Term Support) 버전이다. Java 17은 2021년 9월 14일에 릴리즈되었다.

### 1. Sealed 클래스

- Sealed 클래스는 클래스의 상속을 제한하는 클래스이다. Sealed 클래스는 `sealed` 키워드를 사용하여 선언한다. Sealed 클래스는 `permits` 키워드를 사용하여 허용된 클래스를 선언한다.

```java
public sealed class Shape permits Circle, Rectangle, Triangle {
    // 클래스 내용
}

public final class Circle extends Shape {
    // 클래스 내용
}

public final class Rectangle extends Shape {
    // 클래스 내용
}

public final class Triangle extends Shape {
    // 클래스 내용
}
```

### 2. 패턴 매칭 for instanceof

- Java 17에서는 패턴 매칭을 사용하여 `instanceof` 연산자를 사용할 수 있다. 패턴 매칭을 사용하면 `instanceof` 연산자를 사용할 때, 캐스팅을 자동으로 처리할 수 있다.

```java
public class Main {
    public static void main(String[] args) {
        Object obj = "Hello, World!";
        if (obj instanceof String str) {
            System.out.println(str);
        }
    }
}
```

### 3. Switch 표현식 개선

- Java 17에서는 Switch 표현식에서 `->` 연산자를 사용하여 람다 표현식을 사용할 수 있다.

```java
public class Main {
    public static void main(String[] args) {
        int day = 1;
        String dayOfWeek = switch (day) {
            case 1 -> "Monday";
            case 2 -> "Tuesday";
            case 3 -> "Wednesday";
            case 4 -> "Thursday";
            case 5 -> "Friday";
            case 6 -> "Saturday";
            case 7 -> "Sunday";
            default -> throw new IllegalArgumentException("Invalid day of the week: " + day);
        };
        System.out.println(dayOfWeek);
    }
}
```

### 4. 지역 변수 타입 추론 개선

- Java 17에서는 지역 변수 타입 추론을 개선했다. Java 17에서는 지역 변수 타입 추론을 사용하여 람다 표현식을 사용할 수 있다.

```java
public class Main {
    public static void main(String[] args) {
        var list = List.of(1, 2, 3, 4, 5);
        list.forEach((var i) -> System.out.println(i));
    }
}
```
