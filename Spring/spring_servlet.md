# 서블릿  (Servlet)
- 클라이언트의 요청을 처리하고, 그 결과를 반환하는 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술

<br>

## 서블릿 컨테이너 (Servlet Container)
- 서블릿을 관리하고 실행하는 컨테이너
- 서블릿의 생명주기를 관리하고, 요청을 전달하고, 응답을 받아 클라이언트에게 전달하는 역할을 함

<br>

## 서블릿의 생명주기
- 서블릿 컨테이너에 의해 관리되는 서블릿의 생명주기
- 서블릿 컨테이너는 서블릿의 인스턴스를 생성하고 초기화하며, 서비스 메소드를 호출하고, 소멸시키는 작업을 수행

<br>

## Hello 서블릿 예제

> 스프링 부트는 톰캣 서버를 내장하고 있으므로, 톰캣 서버 설치 없이 편리하게 서블릿 코드를 실행할 수 있다.

### 스프링 부트 서블릿 환경 구성
> `@ServletComponentScan`
>
> 스프링이 자동으로 내 패키지를 포함한 모든 하위패키지에 있는 서블릿을 찾아 자동으로 등록해주기 위해 @ServletComponentScan 어노테이션 추가

```java
@ServletComponentScan // 서블릿 자동 등록
public class ServletApplication {

}
```

### 서블릿 등록하기
> `@WebServlet`
>
> 서블릿으로 등록하기 위하여 어노테이션 추가
> <br> name은 서블릿 이름, urlPatterns는 URL 매핑을 뜻한다.
> 
> "/{urlPatterns}"이 호출되면, 서블릿 컨테이너는 해당 메서드를 실행한다.

HTTP 요청을 통해 매핑된 URL이 호출되면 서블릿 컨테이너는 다음 메서드를 실행한다.

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpSevlet {
    @Override
    protected void service(HttpServletRequest request,HttpServletResponse response) 
	throws ServletException, IOException {
        
        System.out.println("HelloServlet.service");
        System.out.println("request = " + request);
        System.out.println("response = " + response);
    }
}
```

#### 출력 결과

```java
HelloServlet.service
request = org.apache.catalina.connector.RequestFacade@6b3f6e8d
response = org.apache.catalina.connector.ResponseFacade@7b3f6e8d
```

`HttpServletRequest`, `HttpServletResponse`는 인터페이스이다.
WAS 서버들이 Servlet 표준 스펙을 구현하는것 -> 구현체 존재, 찍히는 것은 구현체

#### request 요청 메시지 

```
String username = request.getParameter("username");
```

서블릿은 `쿼리 파라미터`를 편리하게 받아올 수 있다. 


#### response 응답 메시지

```java
response.setContentType("text/plain");
response.setCharacterEncoding("utf-8");
response.getWriter().write("hello " + username); // HTTP 메시지 바디
```

`http://localhost:8080/hello?username=kim` 에 들어가 쿼리파라미터로 받은 값을 출력해보면 정상적으로 값을 받은 것을 확인할 수 있다.

![](/Spring/img/spring_servlet_01.png)


스프링부트를 실행하면 내장 톰캣 서버를 띄워줌

서블릿 컨테이너를 통해 서블릿을 실행, helloServlet 생성됨

![](/Spring/img/spring_servlet_02.png)

웹 브라우저가 HTTP 요청, 응답 메시지를 서버 측에 던져줌

![](/Spring/img/spring_servlet_03.png)

- 서버는  request, response 객체를 만들어서 helloServlet을 호출해줌
거기에 서비스 메서드를 호출하면서 requset, response 객체를 넘겨준다.

- 그 후 서블릿이 종료되고 나가면서 WAS서버가 response정보를 가지고 HTTP 메세지를 만들어서 반환해준다.

- 웹브라우저는 HTTP 메세지를 받아서 화면에 출력해준다.

<br>

## HttpServletRequest
HTTP 요청 메시지를 개발자 대신 파싱하여 `HttpServletRequest` 객체에 담아 제공한다.

HTTP 요청 메시지
```
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded
username=kim&age=20
```

- **START LINE** : HTTP 메소드, URL, 쿼리 스트링, 스키마, 프로토콜
- **헤더** : 헤더 조회
- **바디** : form 파라미터 형식 조회, message body 데이터 직접 조회

### 부가적인 기능
**임시 저장소 기능**
- 해당 HTTP 요청이 시작부터 끝날 때까지 유지되는 임시 저장소 기능
    - 저장 : `request.setAttribute(name, value)`
    - 조회 : `request.getAttribute(name)`

**세션 관리 기능**
- `request.getSession(create:true);`

## HttpServletRequest

```java
@WebServlet(name = "requestHeaderServlet", urlPatterns = "/request-header")
public class RequestHeaderServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        }
}
```

### START LINE정보
```java
request.getMethod() // 메서드
request.getProtocol() // 프로트콜
request.getScheme() // 스키마
request.getRequestURL() // 요청 url
request.getRequestURI()  // 요청 uri
request.getQueryString() // 쿼리 파라미터
request.isSecure() // 보안(https)
```

#### 출력 예시
```
request.getMethod() = GET
request.getProtocol() = HTTP/1.1
request.getScheme() = http
request.getRequestURL() = http://localhost:8080/request-header
request.getRequestURI() = /request-header
request.getQueryString() = username=kim
request.isSecure() = false
```

### 헤더 정보
```
Enumeration<String> headerNames = request.getHeaderNames();
while (headerNames.hasMoreElements()) {
    String headerName = headerNames.nextElement();
    System.out.println(headerName + ": " + headerName);
}
```

```
request.getHeaderNames().asIterator()
                .forEachRemaining(headerName -> System.out.println(headerName + ": " + request.getHeader(headerName)));
```

#### 출력 예시
``` java
host: host // 클라이언트가 요청한 서버의 호스트 이름과 포트 번호
connection: connection // 클라이언트와 서버 간의 연결을 제어하기 위해 사용
content-length: content-length // HTTP 요청 메시지 바디의 길이
cache-control: cache-control // 캐시 제어 ex) no-cache
upgrade-insecure-requests: upgrade-insecure-requests //HTTPS로 전환
user-agent: user-agent // 클라이언트의 정보를 나타냄
accept: accept // 클라이언트가 처리할 수 있는 미디어 타입
accept-encoding: accept-encoding // 클라이언트가 처리할 수 있는 인코딩
accept-language: accept-language // 클라이언트가 처리할 수 있는 언어
cookie: cookie // 클라이언트가 서버에게 전송한 쿠키
sec-ch-ua: sec-ch-ua // 사용자 에이전트의 정보를 나타냄
sec-ch-ua-mobile: sec-ch-ua-mobile // 모바일 기기인지 확인
sec-fetch-site: sec-fetch-site // cross-site 요청인지 확인
sec-fetch-mode: sec-fetch-mode // 요청의 모드를 나타냄
sec-fetch-user: sec-fetch-user // 사용자 정보를 나타냄
sec-fetch-dest: sec-fetch-dest // 요청의 목적지를 나타냄
```

#### 모든 헤더 정보가 아닌 원하는 정보만 얻고 싶을 경우
```java
request.getServerName() // 이름
request.getServerPort() // 포트

request.getLocales().asIterator() // Accept-Language, 언어
                .forEachRemaining(locale -> System.out.println("locale = " + locale));
request.getLocale()

if (request.getCookies() != null) { // 쿠키
            for (Cookie cookie : request.getCookies()) {
                System.out.println(cookie.getName() + ": " + cookie.getValue());
            }
}

request.getContentType() // content 타입
request.getContentLength() // content 길이
request.getCharacterEncoding() // 인코딩
```

#### 출력 예시
```java
request.getServerName() = localhost
request.getServerPort() = 8080

request.getLocale() = ko_KR

request.getContentType() = null
request.getContentLength() = -1
request.getCharacterEncoding() = null
```

#### 그 외 기타정보
```java
request.getRemoteHost()
request.getRemoteAddr()
request.getRemotePort()

request.getLocalName()
request.getLocalAddr()
request.getLocalPort()
```

#### 출력 예시
```java
// Remote(요청한 클라이언트) 정보
request.getRemoteHost() = 0:0:0:0:0:0:0:1
request.getRemoteAddr() = 0:0:0:0:0:0:0:1
request.getRemotePort() = 57984

// Local(서버) 정보
request.getLocalName() = localhost
request.getLocalAddr() = 0:0:0:0:0:0:0:0
request.getLocalPort() = 8080
```

<br>

## HTTP 요청 데이터

HTTP 요청 메시지를 통해 클라이언트에서 서버로 데이터를 전달하는 방법에는 주로 3가지가 있다.

- **GET - 쿼리 파라미터**
    - /url?username=hello&age=20
    - 바디 없이 URL의 쿼리 파라미터에 데이터를 포함해 전달
    - ex) 검색, 필터, 페이징
- **POST - HTML Form**
    - content-type : application/x-www-form-urlencoded
    - 메시지 바디에 쿼리 파리미터 형식으로 전달
    - ex) 회원 가입, 상품 주문, HTML Form
- **HTTP message body**
    - HTTP API에서 주로 사용, JSON, XML, TEXT
    - 데이터 형식은 주로 JSON
    - POST, PUT, PATCH

<br>

## HTTP 요청 데이터 - GET 쿼리 파라미터
메시지 바디없이, URL의 `쿼리 파라미터`를 사용하여 데이터를 전달한다.
 
`?`를 시작으로 보낼 수 있으며, 추가 파라미터는 `&`를 통해 구분된다.

### 1. 파라미터 전송 기능
`http://localhost:8080/request-param?username=hello&age=20`

#### 전체 파라미터 조회
```java
@WebServlet(name="requestParamServlet", urlPatterns = "/request-param")
public class RequestParamServlet extends HttpServlet {
    
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.getParameterNames().asIterator()
                            .forEachRemaining(paramName -> System.out.println(paramName + "=" + request.getParameter(paramName)));
    }
}
```
username은 key, value는 getParameter를 통해 뽑아야함

```
username=hello
age=20
```

#### 단일 파라미터 조회
```java
String username = request.getParameter("username");
String age = request.getParameter("age");
```

```
username = hello
age = 20
```

#### 복수 파라미터 조회
`http://localhost:8080/request-param?username=hello&age=20&username=hello2 `
```java
String[] usernames = request.getParameterValues("username");
for (String name : usernames) {
	System.out.println("username = " + name);
}
```

```
username = hello
username = hello2
```

<br>

## HTTP 요청 데이터 - POST HTML Form

HTML Form을 통해 전달되는 데이터를 받아올 수 있다.

**HTML Form**
- Content-Type: application/x-www-form-urlencoded
- 메시지 바디에 쿼리 파라미터 형식으로 전달
- ex) username=hello&age=20


```html
<form action="/request-param" method="post">
    <input type="text" name="username">
    <input type="text" name="age">
    <button type="submit">전송</button>
</form>
```

`application/x-www-form-urlencoded` 형식은 GET 쿼리 파라미터 형식과 같아, 위에서 만든 쿼리 파라미터 조회 메서드를 그대로 사용할 수 있다.

=> `request.getParameter()` 는 `GET URL 쿼리 파라미터`, `POST HTML Form` 둘 다 지원한다.

> **content-type**은 HTTP 메시지 바디의 데이터 형식을 지정한다.
>
> `GET URL 쿼리 파라미터 형식`은 HTTP 메시지 바디를 사용하지 않기 떄문에 **content-type**이 없다
>
> `POST HTML Form 형식`은 HTTP 메시지 바디에 해당 데이터를 포함하여 보내기 때문에 **content-type**을 지정해주어야 한다. 이렇게 폼으로 데이터를 전송하는 형식을 `application/x-www-form-urlencoded`라고 한다.

<br>

## HTTP 요청데이터 - API 메시지 바디

- HTTP message body에 데이터를 직접 담아서 요청
- HTTP API에서 주로 사용
- POST, PUT, PATCH

### 단순 텍스트 형태
- HTTP 요청 메시지의 메시지 body를 가져온다.
- content-type : text/plain

```java
@WebServlet(name = "messageBodyServlet", urlPatterns = "/message-body")
public class MessageBodyServlet extends HttpServlet {
    
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ServletInputStream inputStream = request.getInputStream();
        inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
        
        System.out.println("messageBody = " + messageBody);
    }
}
```

```java
@PostMapping("/request-body-string-v1")
public void requestBodyString(HttpServletRequest request, HttpServletResponse response) throws IOException {
    ServletInputStream inputStream = request.getInputStream();
    String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
    
    System.out.println("messageBody = " + messageBody);
}
```

```java
@PostMapping("/request-body-string-v2")
public void requestBodyStringV2(@RequestBody String messageBody) {
    System.out.println("messageBody = " + messageBody);
}
```

```java
@PostMapping("/request-body-string-v3")
public HttpEntity<String> requestBodyStringV3(HttpEntity<String> httpEntity) {
    String messageBody = httpEntity.getBody();
    System.out.println("messageBody = " + messageBody);
    return new HttpEntity<>("ok");
}
```

```java
@PostMapping("/request-body-string-v4")
public String requestBodyStringV4(@RequestBody String messageBody) {
    System.out.println("messageBody = " + messageBody);
    return "ok";
}
```

### JSON 형태
- HTTP 요청 메시지의 메시지 body를 가져온다.
- content-type : application/json

```java
public class HelloData {
    private String username;
    private int age;

    // getter

    // setter
}
```
JSON 형태로 파싱할 수 있도록 HelloData라는 객체 생성

```java
@WebServlet(name = "requestBodyJsonServlet", urlPatterns = "/request-body-json")
public class RequestBodyJsonServlet extends HttpServlet {

    // JSON 변환 라이브러리
    private ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletExcetption, IOException {

        HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
    }
}
```
```
{
    "username": "hello",
    "age": 20
}
```

```java
@PostMapping("/request-body-json-v1")
public void requestBodyJsonV1(HttpServletRequest request, HttpServletResponse response) throws IOException {
    ObjectMapper objectMapper = new ObjectMapper();
    ServletInputStream inputStream = request.getInputStream();
    String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
    
    HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
    
    System.out.println("helloData.username = " + helloData.getUsername());
    System.out.println("helloData.age = " + helloData.getAge());
}
```

```java
@PostMapping("/request-body-json-v2")
public void requestBodyJsonV2(@RequestBody HelloData helloData) {
    System.out.println("helloData.username = " + helloData.getUsername());
    System.out.println("helloData.age = " + helloData.getAge());
}
```

> `@RequestBody` : HTTP 요청 메시지의 body에 직접 해당 내용을 입력한다.


<br>

## HttpServletResponse

**HttpServletResponse 역할**
- HTTP 응답 메시지 생성: HTTP 응답코드 지정, 헤더 생성, 바디 생성
- 편의 기능 제공: Content-Type, 쿠키, Redirect

```java
@WebServlet(name = "responseHeaderServlet", urlPatterns = "/response-header")
public class ResponseHeaderServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletExcetption, IOException {

        //[status-line]
        response.setStatus(HttpServletResponse.SC_OK); // http 응답코드 200

        //[response-headers]
        response.setHeader("Content-Type","text/plain;charset=utf-8");
        response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
        response.setHeader("Pragma", "no-cache");
        response.setHeader("my-header","hello");

        //[message body]
        PrintWriter writer = response.getWriter();
        writer.println("ok");
    }
}
```

작성한 응답 데이터들은 Response Headers에서 확인할 수 있다.

```
Content-Type: text/plain;charset=utf-8
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
my-header: hello
```

### 편의 메서드 제공

```java
private void content(HttpServletResponse response) {
    // Content 편의 메서드
    response.setContentType("text/plain");
    response.setCharacterEncoding("utf-8");
}

private void cookie(HttpServletResponse response) {
    // 쿠키 편의 메서드 
    Cookie cookie = new Cookie("myCookie", "good");
    cookie.setMaxAge(600); //600초
    response.addCookie(cookie);
}

private void redirect(HttpServletResponse response) {
    // redirect 편의 메서드
    // response.setStatus(HttpServletResponse.SC_FOUND); // 302
    // response.setHeader("Location", "/basic/hello-form.html");
    response.sendRedirect("/basic/hello-form.html");
} 
```

<br>

## HTTP 응답 데이터 - 단순 텍스트, HTML

### 단순 텍스트 응답
```java
@WebServlet(name = "responseHtmlServlet", urlPatterns = "/response-html")
public class ResponseHtmlServlet extends HttpServlet {

    @Ovierride
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Content-Type : text/html;charset=utf-8
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8")

        PrintWriter writer = response.getWriter();
        writer.println("<html>");
        writer.println("<body>");
        writer.println(" <div>안녕?</div>");
        writer.println("</body>");
        writer.println("</html>");
    }
}
```

### HTTP 응답 데이터 - API JSON

```java
public class HelloData {
    private String username;
    private int age;

    // getter

    // setter
}
```

```java 
@WebServlet(name = "responseJsonServlet", urlPatterns = "/response-json")
public class ResponseJsonServlet extends HttpServlet {

    private ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Content-Type : application/json
        response.setContentType("application/json");
        response.setCharacterEncoding("utf-8");

        HelloData helloData = new HelloData();
        helloData.setUsername("hello");
        helloData.setAge(20);

        String result = objectMapper.writeValueAsString(helloData);
        response.getWriter().write(result);
    }
}
```

```java
@PostMapping("/response-json-v2")
@ResponseBody
public HelloData responseJsonV2() {
    HelloData helloData = new HelloData();
    helloData.setUsername("hello");
    helloData.setAge(20);
    return helloData;
}
```

> `@ResponseBody` : HTTP 응답 메시지의 body에 직접 해당 내용을 입력한다.

<br>

> `application/json`은 스펙 상 utf-8 형식을 사용하도록 정의되어 있다.
> 그래서 스펙에 charset=utf-8과 같은 추가 파라미터를 지원하지 않는다. 따라서 `application/json`이라고만 사용해야지, `application/json;charset=utf-8`이라고 전달하는 것은 의미없는 파라미터를 추가한 것이 된다.