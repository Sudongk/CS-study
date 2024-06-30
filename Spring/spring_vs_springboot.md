# spring vs spring boot

## Spring의 특징

1. DI (Dependency Injection)
spring 프레임워크에 의존성을 주입하면서 객체 간 결합을 느슨하게 하여 코드의 재사용성 증가 및, 단위 테스트 용이
    
    ```java
    public class Car {
        private Engine engine;

        public Car(Engine engine) {
            this.engine = engine;
        }
    }
    ```
2. IoC (Inversion of Control)
의존성 주입의 한 형태로, 객체의 생성 및 생명 주기를 제어하는 책임을 개발자가 아닌 프레임워크에 위임한는 것을 말한다. 개발자는 객체의 구체적인 구현에 의존하지 않고 객체의 인터페이스에만 의존할 수 있다. 이는 코드의 재사용성과 유지보수성을 향상 시킬 수 있다.

    >Spring은 IoC를 구현하는 다양한 방법을 제공하는데 가장 일반적인 방법은 컨테이너에 객체를 등록하고 컨테이너에서 객체를 주입하는 방법이다. 컨테이너는 객체의 생명주기를 관리하고, 개발자는 객체의 인터페이스에만 의존할 수 있다.

3. AOP (Aspect Oriented Programming)
횡단 관심사를 처리하는데 사용됨. 횡단 관심사는 애플리케이션의 주요 기능과 관련이 없는 로깅, 보안 및 트랜잭션 등이 횡단 관심사이다. AOP는 횡단 관심사를 애플리케이션 코드에서 분리하고 프록시 오브젝트를 사용하여 애플리케이션 코드에 추가하는 데 사용 됨.

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

4. Transaction Management
트랜잭션 관리는 데이터베이스 트랜잭션을 처리하는 방법을 제공한다. 스프링은 트랜잭션 관리를 위해 PlatformTransactionManager 인터페이스를 제공하고, 이 인터페이스를 구현하는 여러 클래스를 제공한다.

    ```java
    @Service
    public class EmployeeService {

        @Autowired
        private EmployeeDao employeeDao;

        @Transactional
        public void insertEmployee(Employee employee) {
            employeeDao.insertEmployee(employee);
        }

    }
    ```

5. MVC Framework
스프링은 웹 애플리케이션을 개발하기 위한 MVC 프레임워크를 제공한다. 스프링 MVC는 DispatcherServlet, Controller, Model, View로 구성되어 있다.

    ```java
    @Controller
    public class HelloWorldController {

        @RequestMapping("/")
        public String hello() {
            return "Hello World!";
        }

    }
    ```

## Spring boot의 특징

1. Embed Tomcat을 사용하여 따로 tomcat을 설치하지 않고 jar로 간편하게 배포할 수 있다.

2. starter dependency 제공

- spring boot는 starter dependency를 제공하여 개발자가 필요한 라이브러리를 추가할 때 편리하다. 예를 들어, spring boot web starter를 추가하면 spring mvc, tomcat, jackson 등의 라이브러리를 추가할 수 있다. 

- 이전에는 개발자가 각각의 라이브러리를 추가해야 했지만, starter를 사용하면 필요한 라이브러리를 한번에 추가할 수 있다. 또한, starter는 버전 관리를 해주기 때문에 버전 충돌을 방지할 수 있다.

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ```

    ```gradle
    implementation 'org.springframework.boot:spring-boot-starter-web'
    ```

- spring framework는 각각의 dependency의 호환 버전을 일일이 맞추어야 했고, 하나의 버전을 올리면 다른 dependency에 영향을 미쳐 버전 관리에 어려움이 있었지만 starter가 대부분의 dependency를 관리해주어 편리하다.

3. EnableAutoConfiguration 제공

- spring boot는 EnableAutoConfiguration을 제공하여 개발자가 설정을 최소화할 수 있다. EnableAutoConfiguration은 classpath에 있는 라이브러리를 스캔하여 자동으로 설정을 해준다. 

- 예를 들어, spring boot web starter를 추가하면 tomcat을 내장 서버로 사용하고, spring mvc를 설정해준다. 따라서, 개발자가 별도의 설정을 하지 않아도 웹 애플리케이션을 개발할 수 있다.

4. Actuator 제공

- spring boot는 Actuator를 제공하여 애플리케이션의 상태를 모니터링할 수 있다. Actuator는 애플리케이션의 상태를 확인할 수 있는 여러 엔드포인트를 제공한다. 예를 들어, /health 엔드포인트를 호출하면 애플리케이션의 상태를 확인할 수 있다.

    ```shell
    curl http://localhost:8080/actuator/health
    ```

5. Spring boot CLI 제공

- spring boot CLI(Command Line Interface)는 명령어를 사용하여 spring boot 애플리케이션을 개발할 수 있는 도구이다. CLI를 사용하면 별도의 IDE를 사용하지 않고 명령어로 애플리케이션을 개발할 수 있다.

    ```shell
    spring init --dependencies=web myapp
    ```

6. Spring boot DevTools 제공

- spring boot DevTools는 애플리케이션을 개발할 때 편리한 기능을 제공한다. DevTools를 사용하면 애플리케이션을 다시 시작하지 않고 코드를 수정할 수 있다. 또한, DevTools는 라이브러리를 변경하면 자동으로 애플리케이션을 다시 시작해준다.

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <scope>runtime</scope>
    </dependency>
    ```

    ```gradle
    runtimeOnly 'org.springframework.boot:spring-boot-devtools'
    ```

## 결론

- spring과 spring boot는 모두 java를 기반으로 한 웹 애플리케이션 프레임워크지만 spring boot는 spring을 사용하기 쉽게 만드는 몇가지 기능을 제공한다는 특징이 있다.

- spring은 개발자가 모든 설정을 직접 해야 하지만, spring boot는 starter dependency와 EnableAutoConfiguration을 제공하여 개발자가 설정을 최소화할 수 있다. 또한, spring boot는 Actuator와 DevTools를 제공하여 애플리케이션을 모니터링하고 개발할 때 편리한 기능을 제공한다.