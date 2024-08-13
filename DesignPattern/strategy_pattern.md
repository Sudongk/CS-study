# 전략 패턴 (Strategy Pattern)

## 정의
- 행위 패턴의 하나로, 객체가 할 수 있는 행위를 인터페이스로 정의하고, 행위를 구현한 클래스를 각각 만들어 객체의 행위를 동적으로 바꿀 수 있게 하는 패턴

## 특징

- 행위를 클래스로 캡슐화하여 동적으로 행위를 변경할 수 있음
- 상속을 통한 행위 변경이 아닌, 객체를 통해 행위를 변경
- 전략 패턴을 사용하면 행위를 독립적으로 관리할 수 있음
- 전략 패턴을 사용하면 행위를 쉽게 추가하거나 변경할 수 있음

## 구성요소

- Context: 전략을 사용하는 객체
- Strategy: 전략을 인터페이스로 정의
- ConcreteStrategy: 전략을 구현한 클래스

## 예제

```java
public interface Strategy {
    void runStrategy();
}

public class StrategyA implements Strategy {
    @Override
    public void runStrategy() {
        System.out.println("Strategy A");
    }
}

public class StrategyB implements Strategy {
    @Override
    public void runStrategy() {
        System.out.println("Strategy B");
    }
}

public class Context {
    private Strategy strategy;

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public void runStrategy() {
        strategy.runStrategy();
    }
}

public class Main {
    public static void main(String[] args) {
        Context context = new Context();
        context.setStrategy(new StrategyA());
        context.runStrategy();

        context.setStrategy(new StrategyB());
        context.runStrategy();
    }
}
```

## 장단점

### 장점

- 행위를 독립적으로 관리할 수 있음
- 행위를 쉽게 추가하거나 변경할 수 있음

### 단점

- 전략을 사용하는 객체가 많아질수록 클래스의 수가 늘어남
- 전략을 사용하는 객체가 많아질수록 코드의 복잡도가 증가함
