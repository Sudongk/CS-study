# POJO란 무엇인가?

- POJO(Plain Old Java Object)는 직역하면 간단한 오래된 자바 오브젝트을 뜻합니다. 즉, 다른 환경에 종속되지 않고, 필요에 따라 재사용이 가능한 자바 오브젝트를 뜻합니다.

- Spring 전에 자바 엔트프라이즈 시장은 EJB(Enterprise Java Bean)이 독점하고 있었습니다.

- EJB는 비즈니스 로직을 구현하기 위해 Session Bean, Entity Bean, Message Driven Bean 등을 사용하여 EJB 컨테이너에 의존적인 코드를 작성해야 했습니다.

- 당시 마틴 파울러는 이런한 **문제점을 의식하고, 복잡하고 제한적인 기술보다 자바의 단순 오브젝트를 이용해 비즈니스 로직을 구현하는 편이 낫다**고 생각하여 그것을 뜻하는 `POJO` 를 만들었다.

- POJO를 지키기위해 PSA,IOC,AOP 의 개념을 Spring에 추가하였다.

> POJO 장점
> - 테스트가 용이하다.
> - 코드의 재사용성이 높다.
> - 코드의 가독성이 높아진다.
> - 유지보수가 쉽다.

<br>

# PSA(Portable Service Abstraction)

- PSA는 Spring에서 제공하는 서비스 추상화 계층이다.

- 서비스 추상화란 특정 서비스가 추상화되어있다는 것은 서비스의 내용을 모르더라고 해당 서비스를 이용할 수 있다는 것을 의마한다.

- PSA는 서비스 추상화 계층을 통해 서비스의 구현체를 변경하더라도 비즈니스 로직에 영향을 주지 않는다.

- 예를 들어, MySQL Driver를 사용하다가 Oracle Driver로 변경하여도 비즈니스 로직은 아무 영향을 받지 않고 실행이 됩니다.

## PSA은 왜 사용할까?

- 서비스를 추상화함으로써 개발자가 실제 구현부를 알지 못하더라고 해당 기능을 사용할 수 있게된다. 즉, 추상화 계층인 인터페이스 API의 정보를 활용해 해당 서비스의 모든 기능을 이용하면 된다.

- PSA는 해당 추상화 계층을 구현하는 또 다른 서비스로 언제든지 교체할 수 있게 해준다.

- 예를 들어 Spring 기본으로 톰캣으로 실행이 된다. 그것을 netty 기반으로 실행하는 것도 spring-boot-starter-web 의존성 대신 spring-boot-starter-webflex으로 변경하면 된다.

- 톰캣을 프로젝트에서 netty로 변경하는 것은 복잡한 과정이 있지만, 이것이 추상화가 되고 편하게 사용할수 있게됩니다.

## Spring에서 대표적으로 재공하는 PSA

### Spring Web MVC

- Spring안에는 Servlet을 통해 개발을 할수 있게 많은 기능을 제공해준다.

- Servlet을 통해 프로그램을 실행할려면 클래스에 HttpServlet을 상속박도, doGet(), doPost()을 구현하는 등의 작업을 직접하여야 합니다.

- 하지만, Spring에서는 `@Controller` 와 `@GetMapping`, `@PostMapping` 으로 이러한 어노테이션을 통해 요청을 매핑 할수 있습니다.

### Spring Transaction

- Low level로 트랜잭션을 처리를 하려먼 setAutoCommit()과 commit(), rollback()을 명시적으로 호출해야한다

- 하지만 Spring이 제공하는 @Transactionsl 어노테이션을 메소드에 붙어줌으로써 트랜잭션 처리를 쉽게 해준다.

- 또한 JDBC를 사용하는 DatasourceTransactionManager, JPA를 사용하는 JpaTransactionManager, Hibernate를 사용하는 HibernateTransactionManager를 유연하게 바꿔서 사용할수 있습니다.

- 즉, 구현제가 변경이 되어도 트랙잭션이 처리하는데 전혀 문제가 없습니다

### Spring Cache

- Cache도 Transaction과 동일하게 JCacheManager, ConcurrentMapCacheMannager, EhCacheManager를 사용할수 있습니다.

- 사용자는 @Cacheable 에노테이션을 붙여줌으로써 구현재를 크게 신경쓰지 않아도 필요에 따라 바꿔 쓸수 있다.

<br>

# IOC(Inversion Of Control)

- Spring과 EJB은 개발자가 복잡하고 실수하기 쉬운 로우 레벨의 기술에 많이 신경쓰지 않고도, 애플리케이션의 핵심인 사용자의 요구사향, 즉 비즈니스 로직을 빠르고 효과적으로 구현하게 하는 목표로 했습니다.

- 그러나 EJB는 **메서드나 객체의 호출 작업** 등에 대한 제어를 코드에 침투적으로 반영했고, Spring은 제어와 비지니스 로직을 분리할 수 있게 했다는 차이가 있습니다

- IOC는 해석하면 제어의 역전을 뜻한다.

- 제어는 개발자가 직접 관리를 하는데 이것을 프레임워크에서 객체의 생성과 관리를 하는 것을 의미한다.

- 개발자가 직접 객체를 생성하고 관리하는 코드는 아래와 같이 작성을 합니다

```java
public class A {
	
	private B b;
		
	public A(){
		b = new B();
	}
}
```

위 코드를 보고 “A객체는 B객체에 의존하고 있어” 라고 말할 수 있습니다.

```java
public class A {

	private final B b;

	@Autowired
	private B b;

}
```

- Spring 에서 위와 같은 코드로 의존성이 주입됩니다. 그것을 DI(Dependency Injection)이라고 합니다.

- 즉 외부에서 의존을 주입받으면 DI, 그 의존이 개발자가 아닌 프레임워크에서 관리를 하면 IOC 라고 합니다.

## DI의 방법

의존성을 주입 받는 방법은 3가지가 있습니다.

### Field Injection

가장 흔히 볼 수 있는 Injection방법입니다

```java
@Service
public class A {

    @Autowired
    private B b;

}
```

### Setter Injection

선택적인 의존성을 주입할 경우 유용합니다.

```java
@Service
public class A 
		
    private B b;

	public A() {}

	@Autowired
	public void setA(B b){
		this.b = b;
	}

}
```

### Constructer Injection

```java
@Service
public class A {
	
    private final B b;

	public A(B b) {
		this.b = b;
	}
}
```

<br>

# AOP(Aspect Oriented Programming)

- 관점 지향 프로그래밍이란 객채 지향 프로그램밍의 단점을 보완하기 위해 나온 패러다임이다.

- 객체지향의 가장 큰 장점은 프로그램을 모듈화를 시켜 이를 재활용함으로써 코드의 중복을 줄이고, 재사용성을 높이는 것이었다.

- 하지만 프로그램이 커짐으로서 모듈 안에서 중복되는 코드가 생기게 된다. 그렇게 되면 부분을 수정하면, 다른 클래스에 있는 같은 코드도 수정을 하게됩니다. 즉 유지보수가 어려워지게 된다.

- 이렇게 중복되는 코드를 횡단 관심사(Crosscutting-Concerns)라고 한다. 횡단 관심사는 비즈니스 로직과는 별개로 존재하고, 여러 모듈에서 중복되어 사용된다. 

- AOP의 목적은 횡단 관심사를 모듈화하는 방법을 제시하는 것입니다.

- AOP는 횡단 관심사를 분리하여 모듈화하고, 핵심 비즈니스 로직에서 분리된 횡단 관심사를 필요한 시점에 적용할 수 있게 해줍니다.

- AOP는 주로 로깅, 트랜잭션, 보안, 예외 처리 등에 사용됩니다.

- AOP는 주로 Proxy를 사용하여 구현이 됩니다.
    - Proxy는 대리인이라는 뜻으로, 클라이언트가 서비스를 요청하면, 서비스를 제공하는 객체 대신에 Proxy 객체가 요청을 받아서 처리하는 방식입니다.
    - Proxy 객체는 클라이언트의 요청을 처리하기 전에 추가적인 작업을 수행할 수 있습니다.

- Spring에서는 AOP를 구현하기 위해 `@Aspect` 어노테이션을 사용합니다.

```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.demo.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("logBefore() is running!");
        System.out.println("hijacked : " + joinPoint.getSignature().getName());
        System.out.println("******");
    }
}
```


