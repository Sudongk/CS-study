# 비동기 처리

### 동기와 비동기

**동기 (sync)** : A라는 작업이 끝나는 동시에 B라는 작업을 시작한다

**비동기 (async)** : A라는 작업의 완료 여부와 상관없이 B라는 작업을 시작한다.

>비동기 방식은 작업들이 서로의 작업 시작 및 종료 시간에 영향을 받지 않고, 별도의 작업 시작/종료 시간을 가진다. 그리고 모든 비동기 방식은 멀티 스레드에서 작동한다.

### 동기 비동기 예시 코드

```java
public class SyncAsyncExample {
    public static void main(String[] args) {
        System.out.println("Start");

        // 동기
        System.out.println("동기 작업 시작");
        syncTask();
        System.out.println("동기 작업 끝");

        // 비동기
        System.out.println("비동기 작업 시작");
        asyncTask();
        System.out.println("비동기 작업 끝");

        System.out.println("End");
    }

    public static void syncTask() {
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void asyncTask() {
        new Thread(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
    }
}
```

### 동기 비동기 실행 결과

```
Start
동기 작업 시작
동기 작업 끝
비동기 작업 시작
비동기 작업 끝
End
```

- 동기 작업은 작업이 끝날 때까지 다음 작업을 수행하지 않는다.

- 비동기 작업은 작업이 시작되면 다음 작업을 수행한다.

<br>

### 자바의 비동기 처리

자바에서 비동기 처리를 위해 사용되는 방법은 크게 3가지가 있다.

1. **Future**
2. **CompletableFuture**
3. **Callback**

#### 1. Future

- Future는 자바 5부터 추가된 인터페이스로, 비동기 작업의 결과를 가져오거나 취소할 수 있는 인터페이스이다.

- Future를 사용하면 비동기 작업의 결과를 가져오기 위해 get() 메소드를 사용할 수 있다. 하지만 get() 메소드는 작업이 완료될 때까지 블록킹되므로, 작업이 완료되지 않았을 때 get() 메소드를 호출하면 작업이 완료될 때까지 대기하게 된다.

```java
import java.util.concurrent.*;

public class FutureExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newCachedThreadPool();
        Future<Integer> future = executor.submit(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return 1;
        });

        System.out.println("Started..");

        try {
            System.out.println(future.get());
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }

        executor.shutdown();
    }
}
```
<br>

#### 2. CompletableFuture

- CompletableFuture는 자바 8부터 추가된 클래스로, Future의 단점을 보완한 클래스이다. 

- CompletableFuture는 Future와 달리 작업이 완료되었을 때 콜백을 실행할 수 있다.

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureExample {
    public static void main(String[] args) {
        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return 1;
        });

        System.out.println("Started..");

        future.thenAccept(result -> {
            System.out.println(result);
        });

        future.join();
    }
}
```

<br>

#### 3. Callback

- Callback은 비동기 작업이 완료되었을 때 실행할 콜백을 등록하는 방식이다. 자바에서는 콜백을 
사용하기 위해 인터페이스를 정의하고, 이 인터페이스를 구현한 객체를 등록하는 방식으로 콜백을 사용할 수 있다.

```java
public class CallbackExample {
    public static void main(String[] args) {
        Worker worker = new Worker();
        worker.doWork(new Callback() {
            @Override
            public void onComplete(String result) {
                System.out.println(result);
            }
        });
    }
}

interface Callback {
    void onComplete(String result);
}

class Worker {
    public void doWork(Callback callback) {
        new Thread(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            callback.onComplete("Done");
        }).start();
    }
}
```

### 참고

- [Future (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Future.html)
- [CompletableFuture (Java Platform SE 8 )](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)
- [자바의 비동기 처리](https://www.baeldung.com/java-asynchronous-programming)

