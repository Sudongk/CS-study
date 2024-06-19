# Java Reflection

## Reflection 이란?

- Reflection은 실행 시점에 클래스의 정보를 알아내거나 클래스의 메소드를 호출하거나 필드에 접근할 수 있도록 하는 기능이다.

- Reflection은 클래스의 정보를 알아내는 것이기 때문에 클래스의 이름을 알고 있어야 한다.

- Reflection은 클래스의 정보를 알아내는 것이기 때문에 컴파일 시점에 존재하지 않는 클래스를 사용할 수 있다.

- Reflection은

    - 클래스의 정보를 알아내는 방법
    - 클래스의 생성자를 호출하는 방법
    - 클래스의 메소드를 호출하는 방법
    - 클래스의 필드에 접근하는 방법
    
    등을 제공한다.

## Reflection 사용법

- Reflection을 사용하려면 `java.lang.reflect` 패키지를 사용해야 한다.

- Reflection을 사용하려면 `Class` 객체를 얻어야 한다.

    - `Class` 객체를 얻는 방법
    
        - `Class.forName("패키지명.클래스명")` : 클래스의 이름을 이용해서 `Class` 객체를 얻는다.
        
        - `getClass()` : 객체의 `getClass()` 메소드를 이용해서 `Class` 객체를 얻는다.
        
        - `클래스명.class` : 클래스의 이름을 이용해서 `Class` 객체를 얻는다.

- `Class` 객체를 이용해서 클래스의 정보를 알아낼 수 있다.
    
        - `getName()` : 클래스의 이름을 알아낸다.
        
        - `getSuperclass()` : 클래스의 부모 클래스를 알아낸다.
        
        - `getInterfaces()` : 클래스가 구현한 인터페이스를 알아낸다.
        
        - `getConstructors()` : 클래스의 생성자를 알아낸다.
        
        - `getMethods()` : 클래스의 메소드를 알아낸다.
        
        - `getFields()` : 클래스의 필드를 알아낸다.

- `Class` 객체를 이용해서 클래스의 생성자를 호출할 수 있다.

    - `Constructor` 객체를 이용해서 생성자를 호출한다.
    
    - `Constructor` 객체를 얻는 방법
    
        - `getConstructor(Class... parameterTypes)` : 매개변수 타입을 이용해서 생성자를 얻는다.
        
        - `getConstructors()` : 모든 생성자를 얻는다.
        
    - `Constructor` 객체를 이용해서 생성자를 호출한다.
    
        - `newInstance(Object... args)` : 생성자를 호출한다.

- `Class` 객체를 이용해서 클래스의 메소드를 호출할 수 있다.

    - `Method` 객체를 이용해서 메소드를 호출한다.
    
    - `Method` 객체를 얻는 방법
    
        - `getMethod(String name, Class... parameterTypes)` : 메소드 이름과 매개변수 타입을 이용해서 메소드를 얻는다.
        
        - `getMethods()` : 모든 메소드를 얻는다.
        
    - `Method` 객체를 이용해서 메소드를 호출한다.
    
        - `invoke(Object obj, Object... args)` : 메소드를 호출한다.

- `Class` 객체를 이용해서 클래스의 필드에 접근할 수 있다.

    - `Field` 객체를 이용해서 필드에 접근한다.
    
    - `Field` 객체를 얻는 방법
    
        - `getField(String name)` : 필드 이름을 이용해서 필드를 얻는다.
        
        - `getFields()` : 모든 필드를 얻는다.
        
    - `Field` 객체를 이용해서 필드에 접근한다.
    
        - `get(Object obj)` : 필드 값을 가져온다.
        
        - `set(Object obj, Object value)` : 필드 값을 설정한다.

## Reflection 예제

- `Class` 객체를 얻는 방법

    ```java
    Class<?> clazz = Class.forName("java.lang.String");
    Class<?> clazz = new String().getClass();
    Class<?> clazz = String.class;
    ```

- `Class` 객체를 이용해서 클래스의 정보를 알아내는 방법

    ```java
    Class<?> clazz = Class.forName("java.lang.String");
    System.out.println(clazz.getName());
    System.out.println(clazz.getSuperclass());
    System.out.println(Arrays.toString(clazz.getInterfaces()));
    System.out.println(Arrays.toString(clazz.getConstructors()));
    System.out.println(Arrays.toString(clazz.getMethods()));
    System.out.println(Arrays.toString(clazz.getFields()));
    ```


- `Class` 객체를 이용해서 클래스의 생성자를 호출하는 방법

    ```java
    Class<?> clazz = Class.forName("java.lang.String");
    Constructor<?> constructor = clazz.getConstructor(String.class);
    String str = (String) constructor.newInstance("Hello");
    System.out.println(str);
    ```

- `Class` 객체를 이용해서 클래스의 메소드를 호출하는 방법

    ```java
    Class<?> clazz = Class.forName("java.lang.String");
    Method method = clazz.getMethod("length");
    int length = (int) method.invoke("Hello");
    System.out.println(length);
    ```

- `Class` 객체를 이용해서 클래스의 필드에 접근하는 방법

    ```java
    Class<?> clazz = Class.forName("java.lang.String");
    Field field = clazz.getField("value");
    char[] value = (char[]) field.get("Hello");
    System.out.println(Arrays.toString(value));
    ```

## Reflection 주의사항

- Reflection은 컴파일 시점에 존재하지 않는 클래스를 사용할 수 있기 때문에 컴파일 시점에 오류를 잡을 수 없다.

- Reflection은 실행 시점에 클래스의 정보를 알아내거나 클래스의 메소드를 호출하거나 필드에 접근할 수 있도록 하는 기능이기 때문에 성능이 떨어진다.

## Reflection 사용 예시

- 스프링의 `@Autowired` 어노테이션

    - `@Autowired` 어노테이션은 스프링이 빈을 주입할 때 사용하는 어노테이션이다.
    
    - `@Autowired` 어노테이션은 Reflection을 사용해서 빈을 주입한다.

- JPA의 Entity

    - JPA의 Entity는 기본 생성자를 가져야만 한다.
    
    - JPA는 Reflection을 사용해서 Entity를 생성하기 때문에 기본 생성자를 가져야만 한다.

- JUnit
    
        - JUnit은 Reflection을 사용해서 테스트 메소드를 호출한다.
        
        - JUnit은 Reflection을 사용해서 테스트 메소드를 호출하기 때문에 테스트 메소드는 `public` 이어야 한다.

## Reflection 사용 이유

1. 동적인 객체 생성: Reflection을 사용하면 런타임 시에 클래스의 인스턴스를 동적으로 생성할 수 있습니다. 클래스 이름을 동적으로 전달하여 객체를 생성할 수 있으며, 이는 유연한 객체 생성 패턴을 구현하는 데 도움이 됩니다.

2. 클래스 정보 조사: Reflection은 클래스의 메서드, 필드, 상위 클래스, 인터페이스 등의 정보를 동적으로 조사할 수 있습니다. 이를 통해 프로그램이 실행 중에 클래스의 구조를 분석하고 다양한 작업을 수행할 수 있습니다. 예를 들어, 특정 어노테이션을 가진 메서드를 찾거나, 클래스의 필드 값을 변경할 수 있습니다.

3. 동적 메서드 호출: Reflection을 사용하면 메서드의 이름과 매개변수를 동적으로 지정하여 메서드를 호출할 수 있습니다. 이는 일반적으로 알려지지 않은 클래스나 인터페이스와 상호작용해야 할 때 유용합니다. 예를 들어, 외부 라이브러리나 플러그인 시스템과의 통합에 활용할 수 있습니다.

4. 애노테이션 처리: Reflection을 사용하면 애노테이션 정보를 읽고 처리할 수 있습니다. 런타임 시에 애노테이션을 분석하여 특정 작업을 수행하거나, 애노테이션을 기반으로 동적으로 코드를 생성할 수 있습니다. 이는 프레임워크나 라이브러리에서 커스텀 애노테이션을 활용하여 기능을 확장하거나, 실행 중에 설정 정보를 처리하는 데 사용됩니다.

5. 리소스 접근: Reflection을 사용하면 클래스로더를 통해 리소스에 접근할 수 있습니다. 클래스 경로에 있는 파일이나 설정 파일 등을 동적으로 로드하여 사용할 수 있습니다.