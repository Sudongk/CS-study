# MVC 패턴
- 앞서 살펴본 JSP만으로 구현한 웹앱에서는 비즈니스 로직의 노출, 가독성 저하와 같은 몇가지 문제점이 발생
- 문제 해결을 위해 JSP는 View로직만 처리 하게, 나머지 비즈니스 로직은 서블릿과 같은 다른 

## MVC 패턴

- Model, View, Controller의 각 머릿글자를 따온 표현으로 프로젝트나 앱을 구성할 때 구성요소를 세가지 역할로 구분한 것.
- 각 역할은 다음과 같다.
  - Model: 데이터와 비즈니스 로직을 담당
  - View: 사용자에게 보여지는 화면을 담당
  - Controller: 사용자의 입력을 받아 Model과 View를 제어

  > 비즈니스 로직 제체를 컨트롤러에서 핸들링 하게되면 컨트롤러에 너무 많은 책임이 부과된다. 그래서 일반적으로 비즈니스 로직은 Service 계층을 두어 따로 처리한다.

## MVC 예제

- JSP 파일 경로 : webapp 디렉토리 내 WEB-INF 디렉토리 내 jsp 파일 생성
> - WEB-INF 디렉토리는 외부에서 직접 접근 불가능 <br>
> - 이 경로에 있는 파일은 컨트롤러를 통해 접근

### index.html
    
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
  <h1>index.html</h1>
  <p><a href="/join">add new member</a></p>
  <p><a href="/members">show all members</a></p>
</body>
</html> 
```

### JoinFormServlet

- 새로운 회원을 등록하기위해 값을 입력받는 페이지 서블릿 구현

```java
@WebServlet(name = "joinFormServlet", urlPatterns = "/join")
public class JoinFormServlet extends HttpServlet {

    @Override
    protected void service(
            HttpServletRequest request,
            HttpServletResponse response
    ) throws ServletException, IOException {
        String viewPath = "/WEB-INF/join-form.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

> #### `redirect` vs `forward`
> 리다이렉트는 실제 클라이언트에 응답이 출력되고나서 클라이언트가 `redirect`에 설정된 경로로 다시 요청한다.
> 클라이언트가 인지 할 수 있고, URL경로도 실제로 변경된다.
> `forward`의 경우 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 알 수 없다.

### join-form.jsp

- action 경로 수정
  - "save.jsp" => "save"
  - save.jsp 파일을 요청하는게 아니라 현재 계층 경로 + 작성한 경로로 요청하는것.
  - 요청 경로: `/save`

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>join</title>
</head>
<body>
    <form action="save" method="post">
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

### SaveServlet

- 입력 폼에서 전달 받은 값을 저장하고 그 값을 View 계층으로 전달 하는 로직 구현

```java
@WebServlet(name = "saveServlet", urlPatterns = "/save")
public class SaveServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(
            HttpServletRequest request,
            HttpServletResponse response
    ) throws ServletException, IOException {
        String username = request.getParameter("username");
        Integer age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        Member savedMember = memberRepository.save(member);

        request.setAttribute("member", savedMember);

        String viewPath = "/WEB-INF/save.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

### save.jsp

- 내부 비즈니스 로직 제거
- SaveServlet에서 전달받은 속성 렌더링
- `${}`문법으로 서블릿에서 설정한 속성 렌더링

```html 
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>result</title>
</head>
<body>
    <p>id: ${member.id}</p>
    <p>username: ${member.username}</p>
    <p>age: ${member.age}</p>
    <p><a href="/">back to main</a></p>
</body>
</html>
```

### MemberListServlet

- 회원 목록을 조회하는 서블릿 구현

```java
@WebServlet(name = "memberListServlet", urlPatterns = "/members")
public class MemberListServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(
            HttpServletRequest request,
            HttpServletResponse response
    ) throws ServletException, IOException {
        List<Member> members = memberRepository.findAll();

        request.setAttribute("members", members);

        String viewPath = "/WEB-INF/members.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

### members.jsp

- 회원 목록을 렌더링

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
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
            <c:forEach var="item" items="${members}">
                <tr>
                    <td>${item.id}</td>
                    <td>${item.username}</td>
                    <td>${item.age}</td>
                </tr>
            </c:forEach>
        </tbody>
    </table>
</body>
</html>
```

## 정리

- JSP가 거의 모든 영역의 역할을 담당하던 것에서 코드를 분리해 모델, 컨트롤러, 뷰를 명확하게 분리 가능

### MVC 패턴을 적용해도 남아있는 문제점

#### 1. 중복된 코드가 여전히 많다.

- `JoinFormServlet`, `SaveServlet`, `MemberListServlet` 모두 `MemberRepository`를 사용
- viewPath, dispatcher.forward() 코드가 중복된다.

#### 2.2 유의미하지 않은 코드

- `JoinFormServlet`, `SaveServlet`, `MemberListServlet` 모두 `service()` 메서드를 오버라이딩 하고 있다.
- 모든 서블릿에서 인자값으로 `HttpServletRequest`, `HttpServletResponse`를 가진다.

### 해결방안

- `FrontController`패턴을 도입하여 중복 코드를 제거하고, 공통 로직을 처리할 수 있다.

> `FrontController`패턴
> - 클라이언트의 모든 요청을 하나의 진입점으로 받아서 처리하는 패턴
> - 클라이언트의 요청에 따라 적절한 컨트롤러로 요청을 전달하는 패턴
> - `DispatcherServlet`이 `FrontController`패턴을 구현한 대표적인 서블릿이다.