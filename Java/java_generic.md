# Generic
- 모든 종류의 타입을 다룰 수 있도록, 클래스나 메소드를 일반화된 타입 매개 변수(generic type)를 이용하여 선언하는 기법 

<br>

## Generic 사용법
- 컬렉션 클래스에서 타입 매개 변수로 사용하는 문자는 다른 변수와 혼동을 피하기 위해 일반적으로 하나의 대문자를 사용한다. 
- 아래는 관례적으로 타입매개변수에 많이 사용하는 문자이며, 반드시 일치할 필요는 없다.

| 타입 | 설명 | 
|------|------------|
| **`<T>`** | Type |
| **`<E>`** | Element |
| **`<K>`** | Key |
| **`<V>`** |  Value |
| **`<N>`** | Number |

<br>

## 제네릭 선언
### 1. 클래스 및 인터페이스 선언
```java
public class ClassName <T> { ... }
public Interface InterfaceName <T> { ... }
```
T타입은 해당블럭 { ... } 안에서까지만 유효하다.

### 2. 제네릭 타입 두개
```java
public class ClassName <T, K> { ... }
public Interface InterfaceName <T, K> { ... }

// HashMap의 경우
public class HashMap <K, V> { ... }
```
제네릭 타입을 두개 둘 수도 있다. 대표적으로 `HashMap`이 있다.

### 3. 객체 생성
```java
public class MyClass <T, K> { ... }

public class Main {
    public static void main(String[] args) {
        MyClass<String, Integer> a = new MyClass<Strnig, Integer>();
    }
}
```
- 생성된 제네릭 클래스를 사용해 객체를 생성할때, **구체적인 타입을 명시**해주어야 한다. 
- 위의 예제에 따르면 T는 String이 되고, K는 Integer가 된다.
- 주의 점 : 파라미터로 명시할 수 있는 것은 `참조 타입(Reference Type)`밖에 올 수 없다.
- `primitive type`(ex. int, double, char...)은 올 수 없는 대신 Integer, Double 같은 `Wrapper Class`으로 사용하여야 한다.
- 참조타입이 올 수 있다는 것은 `사용자 정의 클래스`도 타입으로 올 수 있다는 것을 의미한다.

<br>

## [ 제네릭 클래스 ]
```java
class MyClass<K, V> { 
    private K first;
    private V second;

    void set(K first, V second) {
        this.first = first;
        this.second = second;
    }

    K getFirst() {
        return first;
    }

    V getSecond() {
        return second;
    }
 }

public class Main {
    public static void main(String[] args) {
        MyClass<String, Integer> a = new MyClass<String, Integer>();
        
        a.set("hi",10);

        System.out.println("first data : " + a.getFirst());
        System.out.println("K Type : " + a.getFirst().getClass().getName());
        System.out.println("second data : " + a.getSecond());
        System.out.println("V Type : " + a.getSecond().getClass().getName());
    }
}

--------------------------------------------------------

> 출력 결과
first data : hi
K Type : java.lang.String
second data : 10
V Type : java.lang.Integer

```

객체를 생성할 때에 <>안에 `타입 파라미터(Type parameter)`를 지정한다.

만일 MyClass<Integer, String>으로 생성하게 되면, MyClass K V 제네릭 타입은 그에 맞게 변환된다.

<br>

## [ 제네릭 메소드 ]
```java
public <T> T genericMethod(T o) {
    ...
}
```
클래스와 달리 **반환타입 이전에 <> 제네릭 타입을 선언**한다.

<br>

```java
class MyClass<E> {
	
	private E element;	// 제네릭 타입 변수
	
	void set(E element) {	// 제네릭 파라미터 메소드
		this.element = element;
	}
	
	E get() {	// 제네릭 타입 반환 메소드 
		return element;
	}
	
	<T> T genericMethod(T o) {	// 제네릭 메소드
		return o;
	}
 
	
}
 
public class Main {
	public static void main(String[] args) {
		
		MyClass<String> a = new MyClass<String>();
		MyClass<Integer> b = new MyClass<Integer>();
		
		a.set("10");
		b.set(10);
	
		System.out.println("a data : " + a.get());
		// 반환된 변수의 타입 출력 
		System.out.println("a E Type : " + a.get().getClass().getName());
		
		System.out.println();
		System.out.println("b data : " + b.get());
		// 반환된 변수의 타입 출력 
		System.out.println("b E Type : " + b.get().getClass().getName());
		System.out.println();
		
		// 제네릭 메소드 Integer
		System.out.println("<T> returnType : " + a.genericMethod(3).getClass().getName());
		
		// 제네릭 메소드 String
		System.out.println("<T> returnType : " + a.genericMethod("ABCD").getClass().getName());
		
		// 제네릭 메소드 ClassName b
		System.out.println("<T> returnType : " + a.genericMethod(b).getClass().getName());
	}
}

--------------------------------------------------------

> 출력 결과
a data : 10
a E Type : java.lang.String

b data : 10
b E Type : java.lang.Integer

<T> returnType : java.lang.Integer
<T> returnType : java.lang.String
<T> returnType : MyClass

```

- 객체를 생성할 때 <>안에 `타입 파라미터(Type Parameter)`를 지정한다.
    - a 객체의 MyClass E 제네릭 타입은 String으로 모두 변환된다.
    - b 객체의 MyClass E 제네릭 타입은 Integer로 모두 변환된다.
- **genericMethod()는 파라미터 타입에 따라 T타입이 결정된다.**
    - 즉, 클래스에서 지정한 제네릭 유형과 별도로 메소드에서 독립적으로 제네릭 유형을 선언하여 쓸 수 있다.

<br>

위와 같은 방식이 필요한 이유는 **정적 메소드로 선언할 때 필요하기 때문이다.**

앞서 제네릭 유형을 **외부**에서 지정해준다고 하였다. 
<br> 즉, 해당 클래스 객체가 인스턴스화 했을 때, <> 괄호 사이에 파라미터로 넘겨준 타입이 지정된다는 뜻이다.

객체 생성을 통해 접근할 필요 없이 프로그램 실행 시 이미 메모리에 올라가 있기 때문에 클래스 이름을 바로 쓸 수 있다.

<br>

### `static` 메소드는 객체가 생성되기 전에 이미 메모리에 올라가는데, 타입을 어디서 얻어올 수 있을까?

```java
class ErrorClass<E> {
 
	static E genericMethod(E o) {	// error!
		return o;
	}
	
}

public class MyClass<E> { 
	private E element; // 제네릭 타입 변수
 
	void set(E element) { // 제네릭 파라미터 메소드
		this.element = element;
	}
 
	E get() { // 제네릭 타입 반환 메소드
		return element;
	}
 
	// 아래 메소드의 E타입은 제네릭 클래스의 E타입과 다른 독립적인 타입이다.
	static <E> E genericMethod1(E o) { // 제네릭 메소드
		return o;
	}
 
	static <T> T genericMethod2(T o) { // 제네릭 메소드
		return o;
	}
 
}
 
class Main {
 
	public static void main(String[] args) {
 
		// ErrorClass 객체가 생성되기 전에 접근할 수 있으나 유형을 지정할 방법이 없어 에러남
		ErrorClass.getnerMethod(3);

        MyClass<String> a = new MyClass<String>();

        a.set("hi");

        System.out.println(" a data : " + a.get());
        System.out.println("a E Type : " + a.get().getClass().getName());

        // 제네릭 메소드1 Integer
		System.out.println("<E> returnType : " + MyClass.genericMethod1(3).getClass().getName());
 
		// 제네릭 메소드1 String
		System.out.println("<E> returnType : " + MyClass.genericMethod1("ABCD").getClass().getName());
 
		// 제네릭 메소드2 ClassName a
		System.out.println("<T> returnType : " + MyClass.genericMethod2(a).getClass().getName());
 
		// 제네릭 메소드2 Double
		System.out.println("<T> returnType : " + MyClass.genericMethod2(3.0).getClass().getName());
 
	}
}

--------------------------------------------------------

>> 출력결과

a data : hi
a E Type : java.lang.String
<E> returnType : java.lang.Integer
<E> returnType : java.lang.String
<T> returnType : MyClass
<T> returnType : java.lang.Double

```

제너릭 메소드의 제너릭 타입은 지역변수처럼 사용되기 때문에, 프로그램이 실행되어 static 메소드가 메모리에 올라갈 때 타입 지정 없이 메소드의 틀만 공유될 수 있다.

이후, 메소드 호출 시 타입을 지정하면 된다!

**제네릭 메소드는 제네릭 클래스 타입과 별도로 지정된다!!**

클래스와 같은 E 타입이더라도 static 메소드는 객체가 생성되기 이전 시점에 메모리에 먼저 올라가기 때문에 E 유형을 클래스로부터 얻어올 방법이 없다.

<br>

## 제한된 Generic과 와일드 카드
`extends` 와 `super`, `와일드 카드`를 이용

## extends 와 super
외부에서 데이터를 생산한다면(Producer) **extends**를, 외부에서 데이터를 소모한다면(Consumer) **super**를 사용하라.

<br>

```java
<K extends T> // T와 T의 자손 타입만 가능 (K는 들어오는 타입으로 지정 됨)
<K super T>	// <T super [타입]> 은 존재하지않음
 
<? extends T> // T와 T의 자손 타입만 가능
<? super T>	// T와 T의 부모(조상) 타입만 가능
<?> // 모든 타입 가능 <? extends Object>랑 같은 의미
```

<br>

![](/Java/img/java_generic.png)

<br>

- `extends T` : 상한 경계, 뒤에 오는 타입이 최상위 타입으로 한계가 정해진다.
    - < T extends Fruit > : Fruit, Apple 타입만 올 수 있다
    - < T extends Beef > : Beef 타입만 올 수 있다
    - < T extends Food > : Food, Fruit, Apple, Meat, Beef 타입이 올 수 있다
    - 만일 여러 개의 타입을 동시에 상속한 경우로 제한하고 싶다면 `&` 기호를 사용할 수 있다
- `super T` : 하한 경계, 뒤에 오는 타입이 최하위 타입으로 한계가 정해진다.
    - < ? super Fruit > : Fruit, Food 타입만 올 수 있다
    - < ? super Beef > : Beef, Meat, Food 타입만 올 수 있다
    - < ? super Food > : Food 타입만 올 수 있다

<br>

## 와일드 카드
> `<?>`는 제네릭 파라미터의 **타입**보다 제네릭 파라미터를 사용하는 **방법**이 더 중요할 때 사용된다.
> 
>  **Unbounded Wildcard**라 부르며, 특정 타입에 종속되지 않고, 어떠한 타입이든 올 수 있음을 의미한다.
> <br> 여기서 중요한 것은 와일드카드가 any type이 아닌, **unknown type**이라는 점이다.

`< T extends [타입] >` 와 `< ? extends [타입] >` 는 비슷한 구조지만 차이점이 있다.

**유형 경계를 지정**하는 것은 같으나, 경계가 지정되고 **T는 특정 타입**으로 지정이 되지만, **?는 타입이 지정되지 않는다.**

#### <? extends [타입]> ####
- 매개변수의 자료형을 특정 클래스를 상속받은 클래스로만 제한함

#### <? super [타입]> ####
- 매개변수의 자료형을 특정 클래스와 그 클래스의 상위클래스로만 제한함

#### <T extends [타입]> ####
- 상속을 이용해서 T의 자료형을 제한함
- 클래스 선언 시 사용하며, 인스턴스 생성 시 특정 클래스를 상속받은 클래스형만 인스턴스 내부에서 사용할 수 있도록 함
- 특정 인터페이스를 구현한 클래스만 사용하려는 경우에도 사용 가능

<br>

### `<T super [타입]>`
**존재하지 않는 문법!**

`<T super HashMap>`이 있다고 가정하자.

Type Ersure를 통해 Object로 변환되기 때문에 어떤 타입인지 추론되지 않는 T는 결국 Object와 다르지 않다.
<br> 만일 타입정보 소거가 이루어지지않는다고 한들, 타입 파라미터는 클래스와 인터페이스를 가리지 않으므로 T가 어떤 타입이 되는지 모호해지는 문제가 발생한다.

`<T super HashMap>` 에서 계층구조를 올라가면 T는 `AbstractMap`, `Map`, `Cloneable`, `Serializable`, `Object`가 모두 올 수 있게 된다. 
이러한 경우 T를 특정할 수 없게되니 전혀 쓸모없는 코드가 된다.

`<? super HashMap>`과 다르게 T를 사용하는 것은 타입 파라미터에 관심이 있는 경우이므로, Object와 다르지않은 `<T super [타입]>`은 의미가 없고 존재하지도 않는다.

<br>

> **Generic-Type Erasure**
>
> 자바 코드를 컴파일 할 때 타입을 검사하고, 런타임 시 해당 타입을 삭제하는 절차
> <br> 컴파일 시 안정성을 보장받을 수 있다.

<br>

## Generic의 장점
1. 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.
2. 클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다, 관리가 편하다.
3. 비슷한 기능을 지원하는 경우, 코드 재사용성이 높아진다.
