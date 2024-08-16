# 옵저버 패턴 (Observer Pattern)

## 정의

- 행위 패턴의 하나로, 객체 사이에 일대다 의존 관계를 정의하여 어떤 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들이 자동으로 알림을 받고 자동으로 갱신되도록 하는 패턴

- 객체 사이의 의존성을 줄이고 객체 간의 결합도를 낮춤

## 특징

- 객체 간의 의존성을 줄여 유연한 코드를 작성할 수 있음

- 객체 간의 결합도를 낮추어 객체를 재사용하기 쉬움

- 객체 간의 상호작용을 통해 객체 간의 의존성을 줄임

- 객체 간의 상호작용을 통해 객체 간의 결합도를 낮춤

## 구성요소

- Subject: 관찰 대상 객체

- Observer: 관찰자 객체

- ConcreteSubject: Subject를 구현한 클래스

- ConcreteObserver: Observer를 구현한 클래스

## 예제

```java
import java.util.ArrayList;
import java.util.List;

public interface Observer {
    void update();
}

public class ConcreteObserver implements Observer {
    @Override
    public void update() {
        System.out.println("Update");
    }
}

public interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

public class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();

    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }
}

public class Main {
    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();
        ConcreteObserver observer = new ConcreteObserver();

        subject.addObserver(observer);
        subject.notifyObservers();
    }
}
```

