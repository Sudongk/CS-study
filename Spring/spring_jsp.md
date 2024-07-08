# JSP (Java Server Pages)

- JSP는 HTML 내부에 Java 코드를 삽입하여 동적인 웹 페이지를 생성하는 서버 사이드 템플릿 엔진이다.
- JSP는 서블릿을 기반으로 만들어졌으며, 서블릿과 마찬가지로 Java 코드를 사용하여 웹 페이지를 생성한다.
- 최근 스프링 프레임워크 5 버전부터 JSP를 권장하지 않고 스프링 부트 2.0부터는 JSP를 사용할 수 없다. 대신 Thymeleaf나 FreeMarker 같은 템플릿 엔진을 사용하도록 권장한다.

> `권장되지 않는 이유`
> - JSP는 Java 코드를 HTML에 섞어 사용하기 때문에 가독성이 떨어진다.
> - JSP는 서블릿으로 변환되어 실행되기 때문에 서블릿보다 느리다.
> - JSP는 스크립트릿을 사용할 수 있어서 비즈니스 로직이나 데이터베이스 연동 코드가 HTML에 섞이게 된다.
> - 서블릿과 자바코드만을 이용해 HTML 문서를 생성하는것 보단 가독성과 작성 난이도가 쉬워졌음에도 불구하고 여전히 다음과 같은 문제점을 갖고있다.
> - 비즈니스 로직을 비롯한 모든 코드가 JSP에 노출되어있다.

## JSP 예제

### 의존성 추가
```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'

    implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
    implementation 'javax.servlet:jstl'
}
```

### 도메인, Repo 생성

- 회원 도메인

```java
public class Member {

    private Long id;
    private String username;
    private Integer age;

    private Member() {}

    public Member(String username, Integer age) {
        this.username = username;
        this.age = age;
    }

    // getter, setter

}
```

- 회원 Repo

```java
public class MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    private static final MemberRepository instance = new MemberRepository();

    // 싱글톤
    public static MemberRepository getInstance() {
        return instance;
    }

    private MemberRepository() {}

    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);

        return member;
    }

    public Member findById(Long id) {
        return store.get(id);
    }

    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore() {
        store.clear();
    }

}
```

- 테스트 코드

```java
public class MemberRepositoryTest {

    MemberRepository memberRepository = MemberRepository.getInstance();

    @AfterEach
    void afterEach() {
        memberRepository.clearStore();
    }

    @Test
    void 싱글톤_테스트() {
        // given
        MemberRepository memberRepository1 = MemberRepository.getInstance();
        MemberRepository memberRepository2 = MemberRepository.getInstance();

        // when
        System.out.println("memberRepository1 = " + memberRepository1);
        System.out.println("memberRepository2 = " + memberRepository2);

        // then
        assertThat(memberRepository1).isSameAs(memberRepository2);
    }

    @Test
    void 회원_저장() {
        // given
        Member member = new Member("hello", 20);

        // when
        Member savedMember = memberRepository.save(member);

        // then
        Member findMember = memberRepository.findById(savedMember.getId());
        assertThat(findMember).isEqualTo(savedMember);
    }

    @Test
    void 모든_회원_조회() {
        // given
        Member member1 = new Member("member1", 20);
        Member member2 = new Member("member2", 30);

        memberRepository.save(member1);
        memberRepository.save(member2);

        // when
        List<Member> result = memberRepository.findAll();

        // then
        assertThat(result.size()).isEqualTo(2);
        assertThat(result).contains(member1, member2);
    }

    @Test
    void 아이디로_회원_조회() {
        // given
        Member member1 = new Member("member1", 20);
        Member member2 = new Member("member2", 30);

        memberRepository.save(member1);
        memberRepository.save(member2);

        // when
        Member findMember = memberRepository.findById(member1.getId());

        // then
        assertThat(findMember).isEqualTo(member1);
    }

}
```

### JSP 파일 생성

- `.jsp`파일은 그 자체로 하나의 자바 클래스가 된다.
- 어플리케이션을 실행하면 JSP 파일이 서블릿으로 변환되어 실행된다.

#### JSP 태그

- `<%@ ... %>`: 지시자
  - 페이지 속성 설정
- `<%-- ... --%>`: 주석
  - 주석 처리
- `<%! ... %>`: 선언
  - 변수, 메서드 선언
- `<%= ... %>`: 표현식
  - 결과값 출력
- `<% ... %>`: 스크립트
  - Java 코드 삽입
- `<jsp:action>...</jsp:action>`: 액션
  - 페이지 삽입, 공유, 자바빈 사용 등

- `<%@ page contentType="text/html;charset=UTF-8" language="java" %>`
    - JSP 사용 시 첫줄에 선언해야하는 구문
    - 이 문서가 JSP라는 의미

- `<%@ page import="com.example.hello.servlet.domain.Member" %>`
    - Member 클래스를 사용하기 위해 import

- `<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`
    - JSTL 사용을 위한 태그 라이브러리 선언

- `<c:forEach items="${members}" var="member">`
    - JSTL forEach 태그
    - members 컬렉션을 순회하면서 member에 담아서 사용

- `<c:out value="${member.username}" />`
    - JSTL out 태그
    - member의 username을 출력

- `<form action="/save" method="post">`
    - form 태그
    - action: 요청 주소
    - method: 요청 방식

- `<input type="text" name="username" placeholder="username" />`
    - input 태그
    - type: 입력 타입
    - name: 파라미터 이름
    - placeholder: 입력창에 표시할 힌트


### index.jsp
    
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
  <h1>index.html</h1>
  <p><a href="/join-form.jsp">add new member</a></p>
  <p><a href="/members.jsp">show all members</a></p>
</body>
</html>
```

### join-form.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>join</title>
</head>
<body>
    <form action="./save.jsp" method="post">
        <label>
            username:
            <input type="text" name="username" />
        </label>
        <label>
            age:
            <input type="text" name="age" />
        </label>
        <button type="submit">submit</button>
    </form>
    <p><a href="/">back to main</a></p>
</body>
</html>
```

### save.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page import="com.example.demo.member.Member" %>
<%@ page import="com.example.demo.member.Repository" %>
<%
    MemberRepository memberRepository = MemberRepository.getInstance();

    String username = request.getParameter("username");
    Integer age = Integer.parseInt(request.getParameter("age"));

    Member newMember = new Member(username, age);
    Member savedMember = memberRepository.save(newMember);
%>
<html>
<head>
    <title>result</title>
</head>
<body>
    <p>id: <%=savedMember.getId()%></p>
    <p>username: <%=savedMember.getUsername()%></p>
    <p>age: <%=savedMember.getAge()%></p>
    <p><a href="/">back to main</a></p>
</body>
</html>
```

### members.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page import="com.example.demo.member.Member" %>
<%@ page import="com.example.demo.member.Repository" %>
<%@ page import="java.util.List" %>
<%
    MemberRepository memberRepository = MemberRepository.getInstance();
    List<Member> members = repository.findAll();
%>
<html>
<head>
    <title>members</title>
</head>
<body>
    <h1>all members</h1>
    <table>
        <thead>
            <th>id</th>
            <th>username</th>
            <th>age</th>
        </thead>
        <tbody>
        <%
            for (Member member : members) {
                out.write("<tr>");
                out.write("<td>" + member.getId() + "</td>");
                out.write("<td>" + member.getUsername() + "</td>");
                out.write("<td>" + member.getAge() + "</td>");
                out.write("</tr>");
            }
        %>
        </tbody>
    </table>
    <p><a href="/">back to main</a></p>
</body>
</html>
```