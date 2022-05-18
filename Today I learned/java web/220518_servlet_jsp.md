## 서버가 클라이언트 전송 과정 (browser)에 명시해야 할 것

1. 보내는 데이터의 타입
2. charset 정보 (encoding)

### 별도로 명시하지 않으면 응답형식의 기본값으로 처리됨.

**기본값**

1. 기본 문서 타입 text/html
2. 문자셋 정보 8859_1 (한글지원 x)
3. 

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		PrintWriter out = response.getWriter();
		out.print("<body>안녕?</body>");
		out.close();
	}
```
![](https://velog.velcdn.com/images/pdg0526/post/ece024c1-e13a-445b-8c50-73ce230fe75b/image.png)



```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.print("<body>안녕?</body>");
		out.close();
	}
```
![](https://velog.velcdn.com/images/pdg0526/post/e5bffd84-5188-4b07-93f3-19d523d5a6ec/image.png)![](https://velog.velcdn.com/images/pdg0526/post/4a9ad710-acaa-4341-9015-1d118a863cd0/image.png)


`response.setContentType("text/html;charset=utf-8");`

- response header에 문서타입과 인코딩 형식을 담아서 요청→ decoding 형식도 따라서 진행!

### HttpServletRequest request method

```java
package step03.http.req;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/test5")
public class ServletRequest extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public ServletRequest() {
        super();

    }

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.print("Server port : " + request.getServerPort()+ "<br/>");
		out.print("Request URI : " + request.getRequestURI() + "<br/>");
		out.print("Request URL : " + request.getRequestURL() + "<br/>");
		out.print("Context Path : " + request.getContextPath()+ "<br/>");
		out.print("Request Protocol : " + request.getProtocol()+ "<br/>");
		out.print("Request Method : " + request.getMethod()+ "<br/>");
		out.print("Query String : " + request.getQueryString()+ "<br/>");
		out.close();
		
	}
}
```
![](https://velog.velcdn.com/images/pdg0526/post/e82b84e9-b547-4a91-81c6-ad988a659268/image.png)

---

## GET, POST

### GET
![](https://velog.velcdn.com/images/pdg0526/post/335651eb-f123-4475-84f7-17af1d94cdf1/image.png)

![](https://velog.velcdn.com/images/pdg0526/post/9eb0fd02-7432-4143-86bf-29fa780de5df/image.png)


- GET: 주소창(요청라인)에 정보들이 표시됨 (ex) html의 a 태그)
- 데이터의 조회에 사용하기 좋은 메서드

### POST

![](https://velog.velcdn.com/images/pdg0526/post/86df914f-3c7a-441b-9534-13669c811107/image.png)



- PSET: 주소창(요청라인)에 정보들이 표시되지 않음
- 데이터의 수정 및 변경요청 등에 적합한 요청방법

---

# Cookie, Session

> 상태 유지 기술
> 

## HTTP 프로토콜 특징

### 무상태 (stateless)

- 각각의 요청은 개별로 동작 → 로그인 후 다른 페이지를 이동하면 로그아웃이 되어야 함

> 하지만, 다른 페이지로 이동해도 로그인은 지속되어야 하기 때문에 이 상태를 유지해야 한다.
> 

## 상태정보 유지 기술

> 상태 정보의 저장 위치와 저장 기간에  따라 유지하는 기술
> 

### 저장 위치에 따른 분류

1. **Cookie**
    - 상태 정보를 Client(Browser) 단에서 저장
        
        ```java
        package step02.cookie;
        
        import java.io.IOException;
        import java.io.PrintWriter;
        
        import javax.servlet.ServletException;
        import javax.servlet.annotation.WebServlet;
        import javax.servlet.http.Cookie;
        import javax.servlet.http.HttpServlet;
        import javax.servlet.http.HttpServletRequest;
        import javax.servlet.http.HttpServletResponse;
        
        @WebServlet("/setCookie")
        public class SetCookieServlet extends HttpServlet {
        	private static final long serialVersionUID = 1L;
        
        	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        		// TODO Auto-generated method stub
        		
        //		서버가 클라이언트한테 응답 메시지로 보낼 데이터에 대한 부가 정보
        		response.setContentType("text/html;charset=UTF-8");
        		
        //		쿠키 객체 생성: Constructs a cookie with the specified name and value. 
        		Cookie cookie1 = new Cookie("id", "guset");
        		Cookie cookie2 = new Cookie("nickName", "Spiderman");
        		cookie2.setMaxAge(60 * 60 * 3);
        		
        //		응답 정보(response)에 쿠기도 하나 동봉함
        		response.addCookie(cookie1);
        		response.addCookie(cookie2);
        		
        		PrintWriter out = response.getWriter();
        		out.print("서버에서 생성한 쿠키가 클라이언트로 전송");
        		
        		out.close();
        		
        		
        	}
        
        }
        ```
        
![](https://velog.velcdn.com/images/pdg0526/post/6106dee2-f65e-4cde-82cf-ceb08fdfa45e/image.png)


2. **Session**
    - 상태 정보를 Server 측에 저장

### 유지 기간에 따른 분류

---