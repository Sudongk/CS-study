# 디폴트 메서드, 추상 클래스와 인터페이스

## 1. 디폴트 메서드 Default Method

### 1.1 개요

- 자바의 인터페이스는 `자바2`, `자바5`에서도 일부 변화가 있었지만 가장 큰 변화는 Java8에서라고 볼 수 있다.

- `자바8` 이전 대비 변경점으로는 **정적 메서드와 디폴트 메서드를 선언 할 수 있음**에 있다.

- `디폴트 메서드`의 출현으로 기존의 배포된 인터페이스에서 **수정이 어려운 단점을 극복** 할 수 있게 됐다.

- 추가적으로 `자바9`에서는 `private` 메서드를 선언해 외부에서 접근 불가능한 메서드 또한 선언 할 수 있게 됐다.

### 1.2 예제로 알아보기

#### 1.2.1 출현과 기본 사용

```
interface Foo {

  // 추상 메서드, 구현하는 클래스에서 무조건 구현해줘야 하는 메서드.
  String getName();

}

------------------------------------------------------

public class Bar implements Foo {

  private String name;

  // 생성자
  public Bar(String name) {
    this.name = name;
  }

  // 추상 메서드 구현
  @Override
  public String getName() {
    return this.name;
  }

}

------------------------------------------------------

public class Main {
  public static void main(String[] args) {
    Foo foo = new Bar("KIM");
    System.out.println(foo.getName());
  }
}

------------------------------------------------------

Result

>> KIM
```

- 위의 예제에서 인터페이스 `Foo`에 이름을 콘솔에 출력하는 기능을 추가하고싶다고 가정해보자.
- 이전의 방식으로는 인터페이스에 새로운 `printName()` 메서드를 추가하고 인터페이스 `Foo`를 구현한 모든 클래스에서 새로운 메서드를 구현해주어야했다.
- 하지만 디폴트 메서드의 출현으로 다음과 같이 사용 할 수 있게 됐다.

```
interface Foo {

  String getName(); // 추상 메서드, 구현하는 클래스에서 무조건 구현해줘야 하는 메서드.

  void printName() {
    System.out.println(getName());
  }

}

------------------------------------------------------

public class Bar implements Foo {

  private String name;

  // 생성자
  public Bar(String name) {
    this.name = name;
  }

  // 추상 메서드 구현
  @Override
  public String getName() {
    return this.name;
  }

}

------------------------------------------------------

public class Main {
  public static void main(String[] args) {
    Foo foo = new Bar("KIM");
    foo.printName();
  }
}

------------------------------------------------------

Result

>> KIM
```

#### 1.2.2 주의할 점

- 다중상속시 같은 이름의 메서드가 두 인터페이스에 모두 존재할 때, 구현 클래스에서 재정의해서 사용해야 한다.
  - 다음 예제를 살펴 보자.
    ```
    interface Foo {
      void sameNameMethod();
    }

    --------------------------------------------

    interface Bar {
      void sameNameMethod();
    }

    --------------------------------------------

    class Impl implements Foo, Bar {
      ...
    }

    --------------------------------------------

    public class Main {
      public static void main(String[] args) {
        Impl impl = new Impl();
        // 에러
        impl.sameNameMethod();
      }
    }

    --------------------------------------------

    Result

    >> Error : Main inherits unrelated defaults...
    ```
  - 이 때, 구현 하는 클래스에서 메서드를 재정의해 사용해야한다.
    ```
    interface Foo {
      void sameNameMethod();
    }

    --------------------------------------------

    interface Bar {
      void sameNameMethod();
    }

    --------------------------------------------

    class Impl implements Foo, Bar {
      @Override
      publci void sameNameMethod() {
        System.out.println("sameNameMethod called!");
      }
    }

    --------------------------------------------

    public class Main {
      public static void main(String[] args) {
        Impl impl = new Impl();
        impl.sameNameMethod();
      }
    }

    --------------------------------------------

    Result

    >> sameNameMethod called!
    ```

- 디폴트 메서드는 `equals()`, `hashCode()` 같은 **Object**클래스의 메서드를 재정의 할 수 없다.

- 정적 메서드는 재정의 할 수 없다.

- 인터페이스간 상속관계에 따라 우선순위가 달라진다.
  - 만약 `interface A extends B` 처럼 인터페이스 A가 B를 상속받고 B에서 정의된 디폴트 메서드를 재정의 할 경우 A의 디폴트 메서드가 우선순위를 갖는다.
  - 만약 `class C implements A`에서 위처럼 A가 B를 상속받고 그 상속받은 인터페이스를 C가 구현해 디폴트 메서드를 재정의 한 경우 C의 재정의된 메서드가 우선권을 갖는다.

## 2. 추상 클래스 vs 인터페이스

- 사실 추상클래스와 인터페이스는 근본적으로 그 쓰임이 달라 같은 선상에 두고 비교하는것이 적절하지 않아보인다.

- 하지만 사용하는것에 비슷한 점과 인터페이스 또한 일종의 클래스라는점, 그리고 아래에서 다룰 공통점이 있으므로 굳이! 비교 해보자.

### 2.1 공통점

- 스스로 new 연산자를 통해 객체를 생성할 수 없다.

- 추상 메서드를 가진다.

- 자손클래스를 이용해 정의된 추상 메서드들을 구현해야한다.

### 2.2 차이점

#### 2.2.1 용도, 태생

- 추상 클래스
  - 여러 클래스를 추상화해 공통점을 묶은 추상클래스를 상속받아 그 기능을 사용하거나 확장하기 위함.

- 인터페이스
  - 여러 구현체들의 맥락에 맞는 동일한 동작을 보장하기 위한 명세.

#### 2.2.2 다중 상속 가능 여부

- 추상 클래스
  - 다중 상속 불가능

- 인터페이스
  - 다중 상속 가능

#### 2.2.3 선언가능한 메서드

- 추상 클래스
  - 일반 클래스와 동일 하고, 추상 메서드를 선언 할 수 있다.

- 인터페이스 
  - 추상 메서드만 선언할 수 있었으나 자바8 부터 디폴트 메서드, 자바9 부터 정적 메서드까지 선언 가능하게 됨.

#### 2.2.4 변수와 생성자, 메서드

- 추상 클래스
  - 생성자와 변수, 메서드를 모두 가질 수 있다.

- 인터페이스
  - 메서드와 정적 상수만 가질 수 있다.
  - 위에 언급한것 처럼 `자바8` 이전까지는 **추상 메서드만**, `자바9` 이전까지는 **추상 메서드 + 디폴트 메서드**, 그 이후로는 **추상 메서드 + 디폴트 메서드 + private 메서드만** 가질 수 있다.

#### 2.2.5 자식 클래스에서의 구현의 강제성

- 추상 클래스
  - 선택적으로 오버라이딩이 가능하다.

- 인터페이스
  - 정의된 모든 추상메서드를 구현해야 한다.
  - 디폴트 메서드의 경우 선택적으로 구현 가능.

#### 2.2.6 키워드

- 추상 클래스
  - `extends`

- 인터페이스
  - `implements`

### 추상 클래스와 인터페이스의 차이점을 바탕으로 어떤상황에 어떤 것이 더 적합한지 설명해주세요.

- A. 추상 클래스는 연관성 있는 클래스들의 속성을 뽑아 다시한번 추상화 한 클래스입니다.
- 동물을 예로 들면 사자, 호랑이, 개 등을 추상화한 계층으로 포유류라고 정의할 수 있고 닭, 오리, 독수리 등을 추상화 한 계층으로 조류라고 표현 할 수 있습니다.
- 이 상황에서 각각의 동물들을 구현할 때 Walkable, Flyable 등의 인터페이스를 정의해 각 동물의 기능을 명세할 수 있습니다.
- 한마디로 추상클래스는 어떤 객체의 범위를 확장할 때 사용하고, 인터페이스는 명세한 기능을 여러 클래스에서 재사용하여 구현할 수 있을 때 사용합니다.
