## Java 11 특징

### 1. 지역변수 문법 확장

- Java 11에서는 `var` 키워드를 사용하여 지역 변수를 선언할 수 있다. `var` 키워드를 사용하면 변수의 타입을 추론할 수 있다.

```java
var list = new ArrayList<String>();
```

### 2. HTTP 클라이언트 API

- Java 11에서는 HTTP/2 표준을 지원하는 새로운 HTTP 클라이언트 API를 제공한다.

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class Main {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://www.naver.com"))
            .build();
        client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
            .thenApply(HttpResponse::body)
            .thenAccept(System.out::println)
            .join();
    }
}
```

> HTTP/1.1 과 HTTP/2 차이점은 뭘까? : 
> - HTTP/1.1은 요청과 응답이 순차적으로 이루어지는 반면, HTTP/2는 요청과 응답이 동시에 이루어진다.
> - HTTP/1.1은 요청과 응답을 여러 개의 TCP 연결로 처리하는 반면, HTTP/2는 하나의 TCP 연결로 처리한다.
> - HTTP/1.1은 헤더 필드를 압축하지 않는 반면, HTTP/2는 헤더 필드를 압축한다.
> - HTTP/1.1은 요청과 응답을 분리하여 처리하는 반면, HTTP/2는 요청과 응답을 스트림으로 처리한다.
 
### 3. 지연 로깅

- Java 11에서는 지연 로깅을 지원한다. 지연 로깅은 로깅 메시지를 생성하는 작업을 지연시키는 것이다.

```java
import java.util.logging.Logger;

public class Main {
    public static void main(String[] args) {
        Logger logger = Logger.getLogger(Main.class.getName());
        logger.info(() -> "Hello, World!");
    }
}
```

> 지연 로깅을 장점
> - 지연 로깅을 사용하면 로깅 메시지를 생성하는 작업을 지연시킬 수 있다.
> - 지연 로깅을 사용하면 로깅 메시지를 생성하는 작업을 최적화할 수 있다.
> - 지연 로깅을 사용하면 로깅 메시지를 생성하는 작업을 비활성화할 수 있다.

### 4. 문자열 API

- Java 11에서는 문자열 API를 개선했다. Java 11에서는 `isBlank()`, `strip()`, `stripLeading()`, `stripTrailing()`, `repeat(int count)` 메서드를 제공한다.

```java
public class Main {
    public static void main(String[] args) {
        String str = "  Hello, World!  ";
        System.out.println(str.isBlank());
        System.out.println(str.strip());
        System.out.println(str.stripLeading());
        System.out.println(str.stripTrailing());
        System.out.println(str.repeat(2));
    }
}
```

> Java 11에서 추가된 문자열 API
> - `isBlank()` : 문자열이 비어있거나 공백 문자만 포함하고 있는지 확인한다.
> - `strip()` : 문자열의 앞뒤 공백을 제거한다.
> - `stripLeading()` : 문자열의 앞쪽 공백을 제거한다.
> - `stripTrailing()` : 문자열의 뒤쪽 공백을 제거한다.
> - `repeat(int count)` : 문자열을 반복한다.

### 5. 파일 API

- Java 11에서는 파일 API를 개선했다. Java 11에서는 `Files.readString(Path path)` 메서드를 제공한다.

```java
import java.nio.file.Files;

public class Main {
    public static void main(String[] args) throws IOException {
        String str = Files.readString(Path.of("test.txt"));
        System.out.println(str);
    }
}
```

> Java 11에서 추가된 파일 API
> - `Files.readString(Path path)` : 파일의 내용을 문자열로 읽어온다.


### 6. GC 인터페이스

- Java 11에서는 GC(Garbage Collector) 인터페이스를 제공한다. GC 인터페이스를 사용하면 GC를 직접 제어할 수 있다.

```java
import java.lang.management.GarbageCollectorMXBean;

public class Main {
    public static void main(String[] args) {
        List<GarbageCollectorMXBean> gcBeans = ManagementFactory.getGarbageCollectorMXBeans();
        for (GarbageCollectorMXBean gcBean : gcBeans) {
            System.out.println(gcBean.getName());
        }
    }
}
```

> GC 인터페이스를 사용하면 어떤 장점이 있을까? :
> - GC 인터페이스를 사용하면 GC를 직접 제어할 수 있다.
> - GC 인터페이스를 사용하면 GC의 동작을 분석할 수 있다.
> - GC 인터페이스를 사용하면 GC의 성능을 최적화할 수 있다.

