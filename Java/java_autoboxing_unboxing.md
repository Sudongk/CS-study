## Auto Boxing
- **Auto Boxing**은 기본 데이터 유형을 해당 래퍼 클래스로 자동 변환하는 것을 의미
- 코딩 단순화, 개발자의 편의성, 생산성을 높이기 위해 Java 5에 도입된 기능
- **Auto Boxing**을 사용하면 Java 컴파일러는 필요한 경우 **int** 또는 **boolean**과 같은 기본 유형을 **Integer** 또는 **Boolean**과 같은 해당하는 래퍼 클래스로 자동 변환
- 기본 타입과 래퍼 클래스 간의 명시적인 변환 필요성을 없애 오류 가능성을 줄여 코드 가독성 증가

<br>

>  **Primitive Types (기본 타입)**
> <br> 종류 : byte, short, int, long, float, double, char, boolean
> <br> 이러한 기본 타입은 <u>메모리에서 값을 직접 보유</u>하며, <u>래퍼 클래스와는 달리 객체가 아니다.</u>
> <br> 기본 타입은 스택(stack) 메모리에 저장되어 빠르게 액세스할 수 있다.

<br>

### 예시
1. Integer 객체에 int 값을 할당 때 자동으로 **int 값을 해당 Integer 객체로 변환**
2. 기본 타입과 래퍼 클래스 간의 비교 또는 연산 작업을 수행할 때 자동으로 변환

<br>

```
public class AutoboxingExample {
    public static void main(String[] args) {
        // Autoboxing - Integer로 변환
        int num = 42;
        Integer wrappedNum = num; // Autoboxing

        // Autoboxing - int형 10을 Integer로 변환
        Integer result = wrappedNum + 10;

        // Unboxing - Integer -> int
        int unwrappedNum = wrappedNum; // Unboxing

        System.out.println("Wrapped Number: " + wrappedNum);
        System.out.println("Result: " + result);
        System.out.println("Unwrapped Number: " + unwrappedNum);
    }
}
```

<br>

## Unboxing
- 래퍼 클래스 객체를 해당 기본 타입으로 자동 변환해주는 것을 의미 (Autoboxing의 반대)
- 래퍼 클래스 개체에 저장된 값을 추출하여 기본 타입 변수에 할당

<br>

### 예시
```
public class MethodOverloadingExample {
    // int parameter
    public static void printNumber(int num) {
        System.out.println("Printing int: " + num);
    }

    // Integer parameter
    public static void printNumber(Integer num) {
        System.out.println("Printing Integer: " + num);
    }

    public static void main(String[] args) {
        int primitiveNum = 42;
        Integer wrappedNum = 24;

        // printNumber(int num) 호출
        printNumber(primitiveNum); 
        // printNumber(Integer num) 호출
        printNumber(wrappedNum);
    }
}
```

### 결론
- 해당 래퍼 클래스와 기본 타입 간의 수동 변환이 필요하지 않아서 원활하고 편리한 방법을 제공
- Java에서는 Java 컴파일러가 수행
- Autoboxing 및 Unboxing은 래퍼 개체 생성 및 관련 메모리 할당으로 인해 약간의 성능 오버헤드가 발생하지만, 대부분의 경우 성능에 미치는 영향은 미미하며 애플리케이션의 전체 성능에 큰 영향을 미치지 않음
- null값을 처리하거나 == 연산을 사용하여 개체를 비교할 때 예상치 못한 동작 발생 가능