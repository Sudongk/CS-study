# 팩토리 패턴 (Factory Pattern)

## 정의

- 객체 생성을 캡슐화하여 객체를 생성하는 인터페이스를 정의하고, 이를 구현한 클래스에서 객체를 생성하는 패턴

## 특징

- 객체 생성을 캡슐화하여 객체 생성 방식을 변경할 수 있음
- 객체 생성을 캡슐화하여 객체 생성 방식을 동적으로 변경할 수 있음
- 객체 생성을 캡슐화하여 객체 생성 방식을 확장하기 쉬움

## 구성요소

- Factory: 객체 생성을 캡슐화한 인터페이스
- ConcreteFactory: Factory를 구현한 클래스
- Product: 생성할 객체의 인터페이스
- ConcreteProduct: Product를 구현한 클래스

## 예제

```java
public interface Product {
    void printName();
}

public class ConcreteProductA implements Product {
    @Override
    public void printName() {
        System.out.println("Product A");
    }
}

public class ConcreteProductB implements Product {
    @Override
    public void printName() {
        System.out.println("Product B");
    }
}

public interface Factory {
    Product createProduct();
}

public class ConcreteFactoryA implements Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

public class ConcreteFactoryB implements Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductB();
    }
}

public class Main {
    public static void main(String[] args) {
        Factory factoryA = new ConcreteFactoryA();
        Product productA = factoryA.createProduct();
        productA.printName();

        Factory factoryB = new ConcreteFactoryB();
        Product productB = factoryB.createProduct();
        productB.printName();
    }
}
```
