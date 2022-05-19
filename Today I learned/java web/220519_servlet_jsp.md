## equals와 ==

### equals

- 객체가 가지고 있는 실제 값

### ==

- 두 비교 대상이 참조타입일 경우 객체가 가지고 있는 주소 값을 비교

```java
class HelloWorld {
    public static void main(String[] args) {
        
        
        String a = "a";
        String b = a;
     
        String c = new String("a");
        
        System.out.println(a.equals(b)); // true
        System.out.println(a == b); // true
        
        System.out.println(a.equals(c)); // true
        System.out.println(a == c); // false
         
    }
}
```

# 📌Redirect

> 페이지를 이동시키기
> 

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form action = "otherSite">
		<input type = "radio" name = "site" value ="naver"/>네이버<br/>
		<input type = "radio" name = "site" value ="google"/>구글<br/>
		<input type = "submit" value = "이동"/>
	</form>

</body>
</html>
```

```java
package step03.pagemove;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/otherSite")
public class RedirectServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.getWriter().append("Served at: ").append(request.getContextPath());
//		test.html에서 입력받은 값 받기
		String site = request.getParameter("site");
		
//		만약 그 값이 naver이면 naver.com으로 이동. (Redirect)
		if (site.equals("naver")) { response.sendRedirect("http://www.naver.com"); }
//		google이면 goole.com으로 이동
		else if (site.equals("google")) { response.sendRedirect("http://www.google.com"); }
		
	}
}
```

# 📌Forward

→ 페이지는 이동하지 않는데, 다른 결과들만 이동시키는 것

```java
@WebServlet("/page1")
public class DispatcherServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		response.setContentType("text/html;charset=UTF-8");
		
		PrintWriter out = response.getWriter();
		out.print("<h3>DispatcherServler 수행결과</h3>");

		ServletContext sc = this.getServletContext();
		RequestDispatcher rd = sc.getRequestDispatcher("/pages2");
		rd.forward(request, response);
		 
		out.close();
	}

}
```

- DispatcherServlet.java

```java
@WebServlet("/page2")
public class DispatcherServlet2 extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=UTF-8");
		
		PrintWriter out = response.getWriter();
		out.print("<h3>DispatcherServler 2 수행결과</h3>");
		
		out.close();

	}

}
```

- DispatcherServlet2.java

DispatcherServlet.java 실행 시 

![](https://velog.velcdn.com/images/pdg0526/post/58191463-1271-4343-930c-67b583d34a54/image.png)


/page1 에서 /page2의 결과가 나오는 것을 알 수 있다. 

---

# 📌JSP

> Java  Server page의 약자로, HTML에 Java 코드를 넣어 동적 웹페이지를 생성하는 웹 애플리캐이션 도구!
> 

## EL

> JSP의 가독성이 너무 안좋아서 등장한 기술로, html 안에 java 코드를 쓸 수 있는 기술이다.
> 

`<% %>` 안에 자바 코드를 넣어서 쓴다

`<%= 변수 %>` 를 써서 변수를 사용할 수 있다. 

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<% // 이 영역 내에선 자바 문법을 따르고, 동적으로 동작하는 코드 ㄱㄴ
		String user = request.getParameter("name");
		if (user == null) { user = "Guest";}
	%>
	
	<h1>Hello, <%= user %>!</h1>

</body>
</html>
```
![](https://velog.velcdn.com/images/pdg0526/post/5eb8013d-ecae-44cc-acaa-85818e75d798/image.png)

![](https://velog.velcdn.com/images/pdg0526/post/bc23ef17-2bcd-4aee-94ee-bd24543a076f/image.png)

- queryString에 GET 방식으로 name값을 변경하면 동적으로 적용된다.

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<%
		String name2 = request.getParameter("name2");
	
	%>
	Hello, <%= name2 %>!<br/>
	Hello, ${ param.name }<br/>

</body>
</html>
```

![](https://velog.velcdn.com/images/pdg0526/post/98d68962-c9a7-4a14-ae1d-105cb5de798c/image.png)


![](https://velog.velcdn.com/images/pdg0526/post/ea71f587-7526-4a0f-8217-a64c48083b04/image.png)

- GET method의 경우 &을 이용해서 다수의 변수에 값을 할당

