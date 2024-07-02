# 서블릿  (Servlet)
- 클라이언트의 요청을 처리하고, 그 결과를 반환하는 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술

## 서블릿 컨테이너 (Servlet Container)
- 서블릿을 관리하고 실행하는 컨테이너
- 서블릿의 생명주기를 관리하고, 요청을 전달하고, 응답을 받아 클라이언트에게 전달하는 역할을 함

## 서블릿의 생명주기
- 서블릿 컨테이너에 의해 관리되는 서블릿의 생명주기
- 서블릿 컨테이너는 서블릿의 인스턴스를 생성하고 초기화하며, 서비스 메소드를 호출하고, 소멸시키는 작업을 수행

## Hello 서블릿 예제

> 스프링 부트는 톰캣 서버를 내장하고 있으므로, 톰캣 서버 설치 없이 편리하게 서블릿 코드를 실행할 수 있다.

### 스프링 부트 서블릿 환경 구성
> `@ServletComponentScan`
>
> 스프링이 자동으로 내 패키지를 포함한 모든 하위패키지에 있는 서블릿을 찾아 자동으로 등록해주기 위해 @ServletComponentScan 어노테이션 추가

```
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

```
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

```
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

## HttpServletRequest
> `HttepServletRequest`
>
> HTTP 요청 메시지를 개발자 대신 파싱하여 `HttpServletRequest` 객체에 담아 제공한다.

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

## HttpServletRequest 기본 사용방법

```
@WebServlet(name = "requestHeaderServlet", urlPatterns = "/request-header")
public class RequestHeaderServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        }
}
```

### START LINE정보
```
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
``` 
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
```
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
```
request.getServerName() = localhost
request.getServerPort() = 8080

locale = ko_KR
locale = ko
locale = en_US
locale = en
request.getLocale() = ko_KR

request.getContentType() = null
request.getContentLength() = -1
request.getCharacterEncoding() = null
```

#### 그 외 기타정보
```
request.getRemoteHost()
request.getRemoteAddr()
request.getRemotePort()

request.getLocalName()
request.getLocalAddr()
request.getLocalPort()
```

#### 출력 예시
```
// Remote(요청한 클라이언트) 정보
request.getRemoteHost() = 0:0:0:0:0:0:0:1
request.getRemoteAddr() = 0:0:0:0:0:0:0:1
request.getRemotePort() = 57984

// Local(서버) 정보
request.getLocalName() = localhost
request.getLocalAddr() = 0:0:0:0:0:0:0:0
request.getLocalPort() = 8080
```



