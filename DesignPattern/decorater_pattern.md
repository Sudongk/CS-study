# 데코레이터 패턴 (Decorator Pattern)

## 정의

- 객체 구조를 유연하게 확장할 수 있는 구조 패턴

## 특징

- 객체에 동적으로 새로운 책임을 추가할 수 있음
- 상속을 통해 기능을 확장하는 방법보다 유연하게 기능을 확장할 수 있음

## 구성요소

- Component: 기본 기능을 정의하는 인터페이스
- ConcreteComponent: 기본 기능을 구현한 클래스
- Decorator: Component를 상속받아 새로운 기능을 추가하는 클래스
- ConcreteDecorator: Decorator를 상속받아 새로운 기능을 구현한 클래스

## 예제

```java
public interface Component {
    void operation();
}

public class ConcreteComponent implements Component {
    @Override
    public void operation() {
        System.out.println("ConcreteComponent operation");
    }
}

public class Decorator implements Component {
    private Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    @Override
    public void operation() {
        component.operation();
    }
}

public class ConcreteDecoratorA extends Decorator {
    public ConcreteDecoratorA(Component component) {
        super(component);
    }

    @Override
    public void operation() {
        super.operation();
        System.out.println("ConcreteDecoratorA operation");
    }
}

public class ConcreteDecoratorB extends Decorator {
    public ConcreteDecoratorB(Component component) {
        super(component);
    }

    @Override
    public void operation() {
        super.operation();
        System.out.println("ConcreteDecoratorB operation");
    }
}

public class Main {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        component.operation();

        Component decoratorA = new ConcreteDecoratorA(new ConcreteComponent());
        decoratorA.operation();

        Component decoratorB = new ConcreteDecoratorB(new ConcreteComponent());
        decoratorB.operation();
    }
}
```
