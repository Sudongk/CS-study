## Java 클래스 로더

클래스 로더(Class Loader)는 Java 가상 머신(JVM)에서 클래스 파일을 동적으로 로드하는 역할을 담당합니다. Java 어플리케이션은 클래스 로더를 사용하여 필요한 클래스를 찾고, 로드하고, 초기화할 수 있습니다.

## 클래스 로더 주요 기능

- ### 로딩(Loading)
  - 바이트 코드(클래스 파일 .class)를 메서드 영역에 로드
  - JVM은 메서드 영역에 읽어온 바이트 코드들의 정보를 저장
    - 로드된 클래스와 그 부모 클래스 정보
    - 클래스 파일과 Class, Interface, Enum 관련 정보
    - 변수나 메서드 정보
- ### 링크(Linking)
  - 검증
    - 클래스 파일의 구조와 바이트 코드가 유효한지 확인
  - 준비
    - 클래스가 필요로 하는 메모리를 할당하고 정의된 클래스, 인터페이스, 필드, 메서드를 나타내는 데이터 구조 준비
  - 분석
    - 심볼릭 메모리 레퍼런스를 메서드 영역 내 실제 레퍼런스로 변환
- ### 초기화(Initialization)
  - 클래스 변수들을 적절한 값으로 초기화 한다. -> 정적 필드를 설정된 값으로 초기화 한다.

## 종류

### 부트스트랩 클래스 로더

클래스 로더 계층 구조 상 최상위 계층 클래스 로더.  
자바 클래스를 로드 할 수 있는 자바 자체의 클래스 로더와 Object 클래스를 로드한다.

### 확장 클래스 로더

부트스트랩 클래스 로더의 자식 클래스 로더로 확장 자바 클래스를 로드한다.

### 시스템 클래스 로더

사용자가 지정한 ${CLASSPATH}의 클래스를 로드한다.

### 사용자 정의 클래스 로더

클래스 로더 중 최하위 계층으로, 애플리케이션 레벨에서 사용자가 직접 정의하고 생성한 클래스 로더.

## 동작 방식

새로운 클래스를 로드할 경우 다음과 같은 절차를 따른다.

- 메서드 영역에 클래스가 적재돼있는지 확인한다. 있다면 그대로 사용한다.
- 메서드 영역에서 찾을 수 없을 경우 `시스템 클래스 로더`에 요청하고 `시스템 클래스 로더`는 `확장 클래스 로더`에 요청을 넘긴다.
- `확장 클래스 로더`는 `부트스트랩 클래스 로더`에 요청을 넘기고 `부트스트랩 클래스 로더`는 `부트스트랩 경로(/jre/lib)`에 해당 클래스가 존재하는지 확인한다.
- 클래스가 없다면 다시 `확장 클래스 로더`로 요청을 넘기고 확장 클래스 로더는 확장 경로(/jre/lib/ext)에서 찾아본다.
- `확장 클래스 로더`에서도 찾을 수 없다면 `시스템 클래스 로더`에서 시스템 경로에서 찾는다. 이 때 찾을 수 없다면 `ClassNotFoundException`을 발생시킨다.

## 클래스 로더 3원칙

### 위임 원칙

클래스 로더는 리소스를 찾을 때 상위 클래스 로더로 요청을 위임한다.

## 가시 범위 언칙

- 하위 클래스 로더는 상위 클래스 로더가 로드한 클래스를 알 수 있지만 상위 클래스 로더는 하위 클래스 로더가 로드한 클래스를 알 수 없다.
- 상위 클래스 로더에서 로드한 클래스를 하위 클래스 로더가 사용할 수 있다.

### 3.3 유일성 원칙

- 하위 클래스 로더는 상위 클래스 로더가 로드한 클래스를 다시 로드하면 안된다.
- 위임 원칙으로 고유한 클래스 로딩을 보장 할 수 있다.

## 동적 로당

### 로드타임 동적 로딩

```
public Class HelloWorld {
    public static void main(String[] args) {
        System.out.println("hello world!!!");
    }
}
```

위처럼 `hello world!!!`를 출력 하는 코드를 컴파일 과정 이후 실행 했을 때 클래스 로더는 `Object` 클래스와 `HelloWorld` 클래스를 읽고 로딩하기 위해 필요한 `String`, `System` 클래스를 로딩한다. 여기서 모든 클래스는 `HelloWorld` 클래스가 실행 되기 전 **로드타임에 동적으로 로드** 된다.

### 런타임 동적 로딩

```
public class Main {

    public static void main(String[] args) {
        System.out.println("Hello world!");
        Car car = new SportsCar();
        car.move();
    }
}

---

public interface Car {

    void move();
}

---

public class SportsCar implements Car {

    @Override
    public void move() {
        System.out.println("Sports car move!!!");
    }
}
```

많은 클래스들이 로드타임에 로드되고 나서야 프로그램이 실행되고 실행되는 런타임에 `SportsCar` 클래스가 로드된다.
상위 클래스인 `Car`는 구체화된 클래스로 객체를 전달 받아야 하는데 프로그램이 실행되며 실행문이 해석되기 전까지 JVM은 `Car`에 `SportsCar`가 올지 `Sedan`이 올지 알 수 없다. 즉, 로드타임에 어떤 클래스를 로딩할지 결정 할 수 없어서 `SportsCar` 클래스는 런타임 중에 로딩된다.

## 동적로딩의 장점과 단점

### 장점

- 런타임 전 까지 메모리 낭비 방지
- 해당 클래스가 사용되는 시점까지 정보를 알 수 없음

### 단점
- 조기에 런타임에러 감지 어려움
- 실행문이 실행되는 시점까지 가서야 발견 가능
- 동적로딩에도 시간비용이 발생하기 때문에 성능저하 발생 가능