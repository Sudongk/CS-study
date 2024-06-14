# CountdownLatch, CyclicBarrier

## CountdownLatch

CountdownLatch는 하나 이상의 스레드가 다른 스레드가 수행하는 작업이 끝날 때까지 기다리도록 하는 클래스이다.

CountdownLatch는 다음과 같은 메소드를 제공한다.

- `CountdownLatch(int count)`: count만큼의 작업이 끝날 때까지 기다리도록 하는 CountdownLatch를 생성한다.

- `void await()`: count만큼의 작업이 끝날 때까지 기다린다. count가 0이 될 때까지 블록된다.

- `void countDown()`: 작업이 끝났음을 알린다. count를 감소시킨다.

### CountDownLatch 작동 원리

CountDownLatch는 count만큼의 작업이 끝날 때까지 기다리도록 한다.

count만큼의 작업이 끝나면 countDown() 메소드를 호출하여 count를 감소시킨다.

count가 0이 되면 await() 메소드를 호출한 스레드가 블록에서 풀리고 다음 작업을 수행한다.

### CountdownLatch 예시 코드

```java
import java.util.concurrent.CountDownLatch;

public class CountDownLatchExample {
    public static void main(String[] args) {
        CountDownLatch latch = new CountDownLatch(3);

        new Thread(() -> {
            try {
                Thread.sleep(1000);
                System.out.println("Thread 1 작업 완료");
                latch.countDown();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();

        new Thread(() -> {
            try {
                Thread.sleep(2000);
                System.out.println("Thread 2 작업 완료");
                latch.countDown();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();

        new Thread(() -> {
            try {
                Thread.sleep(3000);
                System.out.println("Thread 3 작업 완료");
                latch.countDown();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();

        try {
            latch.await();
            System.out.println("모든 작업 완료");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

### 실행 결과

```
Thread 1 작업 완료
Thread 2 작업 완료
Thread 3 작업 완료
모든 작업 완료
```

- CountDownLatch를 생성할 때 count를 3으로 설정하였으므로, 3개의 작업이 끝날 때까지 기다린다.

- 3개의 작업이 끝나면 "모든 작업 완료"를 출력한다.  

<br>

## CyclicBarrier

CyclicBarrier는 여러 스레드가 서로 작업을 완료할 때까지 기다리도록 하는 클래스이다.

CyclicBarrier는 다음과 같은 메소드를 제공한다.

- `CyclicBarrier(int parties)`: parties만큼의 스레드가 작업을 완료할 때까지 기다리도록 하는 CyclicBarrier를 생성한다.

- `int await()`: 스레드가 작업을 완료할 때까지 기다린다. 모든 스레드가 작업을 완료하면 다음 작업을 수행한다.

### CyclicBarrier 작동 원리

CyclicBarrier는 parties만큼의 스레드가 작업을 완료할 때까지 기다리도록 한다.

parties만큼의 스레드가 작업을 완료하면 다음 작업을 수행한다.

### CyclicBarrier 예시 코드

```java
import java.util.concurrent.BrokenBarrierException;

public class CyclicBarrierExample {
    public static void main(String[] args) {
        CyclicBarrier barrier = new CyclicBarrier(3, () -> {
            System.out.println("모든 작업 완료");
        });

        new Thread(() -> {
            try {
                Thread.sleep(1000);
                System.out.println("Thread 1 작업 완료");
                barrier.await();
            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        }).start();

        new Thread(() -> {
            try {
                Thread.sleep(2000);
                System.out.println("Thread 2 작업 완료");
                barrier.await();
            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        }).start();

        new Thread(() -> {
            try {
                Thread.sleep(3000);
                System.out.println("Thread 3 작업 완료");
                barrier.await();
            } catch (InterruptedException | BrokenBarrierException e) {
                e.printStackTrace();
            }
        }).start();
    }
}
```

### 실행 결과

```
Thread 1 작업 완료
Thread 2 작업 완료
Thread 3 작업 완료
모든 작업 완료
```

- CyclicBarrier를 생성할 때 parties를 3으로 설정하였으므로, 3개의 스레드가 작업을 완료할 때까지 기다린다.

- 3개의 스레드가 작업을 완료하면 "모든 작업 완료"를 출력한다.

<br>

## CountDownLatch vs CyclicBarrier

CountDownLatch와 CyclicBarrier는 모두 여러 스레드가 작업을 완료할 때까지 기다리도록 하는 클래스이다.

CountDownLatch와 CyclicBarrier의 차이점은 다음과 같다.

- `CountDownLatch`: count만큼의 작업이 끝날 때까지 기다리도록 한다. count가 0이 되면 더 이상 기다리지 않는다.

- `CyclicBarrier`: parties만큼의 스레드가 작업을 완료할 때까지 기다리도록 한다. parties만큼의 스레드가 작업을 완료하면 다음 작업을 수행한다.

따라서 CountDownLatch는 한 번만 사용할 수 있지만, CyclicBarrier는 여러 번 사용할 수 있다.

CountDownLatch는 작업이 끝날 때까지 기다리는 스레드가 다음 작업을 수행할 수 있지만, CyclicBarrier는 모든 스레드가 작업을 완료해야 다음 작업을 수행한다.

## 정리

- CountdownLatch는 count만큼의 작업이 끝날 때까지 기다리도록 하는 클래스이다.

- CyclicBarrier는 parties만큼의 스레드가 작업을 완료할 때까지 기다리도록 하는 클래스이다.

## 실무 적용 예시

- 여러 스레드가 작업을 완료한 후에 다음 작업을 수행해야 할 때 사용한다.

- 여러 스레드가 작업을 완료한 후에 결과를 모아서 처리해야 할 때 사용한다.

- 여러 스레드가 작업을 완료한 후에 결과를 모아서 처리한 후에 다음 작업을 수행해야 할 때 사용한다.
