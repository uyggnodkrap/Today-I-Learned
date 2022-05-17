# Web server VS WAS

## Web server

> http 기반 사용자의 요청을 응답
> 
- 정적 컨텐츠, 동적 컨텐츠 모두 서비스 할 수 있다.
- ex) Apache, Nginx

## WAS

> 동적 컨텐츠 제공을 위해 만들어진 애플리케이션 서버
> 
- 동적 컨텐츠를 제공하기 위해 만들어진 애플리캐이션 서버
- 웹 컨테이너 또는 **서블릿(servlet)** 컨테이너라고 불림
- DB, 비즈니스 로직 등을 수행

### 구분 이유

> 정적 기능은 web server, 동적 기능은 was에게 위임하여 서버에 부담을 덜기 위함
> 

# Servlet

> 동적 웹 페이지를 만들 때 사용되는 자바 기반의 웹 애플리케이션 프로그래밍 기술
> 
- Java에서 servlet API를 상속받는 것으로 사용할 수 있다.
    - Servlet interface의 HttpServlet을 상속받음

## servlet 특징

- 사용자 요청에 동적으로 작동한다.
- HTML을 사용하여 응답한다.
- Java의 스레드를 이용한다.
- MVC 패턴에서 Controller의 역할을 한다.

## servlet 생명주기

| 메서드명 | 의미 | DB와의 연관성 |
| --- | --- | --- |
| GET | 자원 획득 | Read |
| POST | 자원 생성, 추가 | Create |
| PUT | 자원 변경 | Update |
| DELETE | 자원 삭제 | Delete |

## JSP

> HTML 코드에 Java 코드를 넣어 동적 웹페이지를 생성하는 웹 애플리케이션 도구
> 
- .jsp는 서버로 요청시 servlet으로 파일이 변환되어 html을 반환한다