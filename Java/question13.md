### Spring Web MVC의 Dispatcher Servlet의 동작 원리에 대해서 설명해주세요.
- 우선 MVC는 디자인 패턴이다. Model-View-Controller의 약자로 각각의 역할을 분리한 디자인 패턴을 말한다!
![image](https://github.com/user-attachments/assets/40874fa1-cd62-47d0-9131-a162416232de)
- 요래 생겼다.
- 소프트웨어를 구성하는 요소들을 분리함으로써 코드의 재사용성과 유지보수성을 높이고, 협업을 용이하게 한다.

#### Spring Web MVC??
```
Spring Web MVC is the original web framework built on the Servlet API and
has been included in the Spring Framework from the very beginning.
The formal name, "Spring Web MVC," comes from the name of its source module (spring-webmvc),
but it is more commonly known as "Spring MVC".
Parallel to Spring Web MVC,
Spring Framework 5.0 introduced a reactive-stack web framework whose name, "Spring WebFlux," is also based on its source module (spring-webflux).
This chapter covers Spring Web MVC. For reactive-stack web applications, see Web on Reactive Stack.
For baseline information and compatibility with Servlet container and Jakarta EE version ranges,
see the Spring Framework Wiki.

...

Spring MVC, as many other web frameworks, is designed around the front controller pattern where a central Servlet,
the DispatcherServlet, provides a shared algorithm for request processing,
while actual work is performed by configurable delegate components.
This model is flexible and supports diverse workflows.

The DispatcherServlet, as any Servlet, needs to be declared and mapped according to the Servlet specification
by using Java configuration or in web.xml.
In turn, the DispatcherServlet uses Spring configuration to discover the delegate components it needs for request mapping,
view resolution, exception handling, and more.
```
- 요래 공식문서에 나와 있다.
공식문서: https://docs.spring.io/spring-framework/reference/web/webmvc.html

```
Spring Web MVC는 서블릿 API를 기반으로 구축된 최초의 웹 프레임워크이며,
처음부터 Spring 프레임워크에 포함되어 있습니다. 정식 명칭인 “Spring Web MVC”는 소스 모듈의 이름(spring-webmvc)에서 유래했지만,
 더 일반적으로는 “Spring MVC”로 알려져 있습니다.

스프링 프레임워크 5.0은 스프링 웹 MVC와 병행하여 반응형 스택 웹 프레임워크인 '스프링 웹플럭스'를 도입했는데,
이 이름 역시 소스 모듈(spring-webflux)을 기반으로 합니다.
이 장에서는 Spring Web MVC를 다룹니다.
리액티브 스택 웹 애플리케이션에 대해서는 리액티브 스택의 웹을 참조하십시오.

다른 많은 웹 프레임워크와 마찬가지로 Spring MVC는 중앙 서블릿인 DispatcherServlet이 요청 처리를 위한 공유 알고리즘을 제공하고
실제 작업은 구성 가능한 델리게이트 구성 요소에서 수행하는 프론트 컨트롤러 패턴을 중심으로 설계되었습니다.
이 모델은 유연하며 다양한 워크플로우를 지원합니다.

다른 서블릿과 마찬가지로 DispatcherServlet은 Java 구성을 사용하거나
web.xml에서 서블릿 사양에 따라 선언하고 매핑해야 합니다.
그런 다음 DispatcherServlet은 Spring 구성을 사용하여 요청 매핑, 보기 확인, 예외 처리 등에 필요한 델리게이트 구성 요소를 검색합니다.
```
- ...그렇다고 한다.
- 간단하게 말해보자면, Spring에서 디자인 패턴을 적용하여 HTTP 요청을 효율적으로 처리하고 있다. 정도로 이해할 수 있을 것 같다.

#### DispatcherServlet이요?
- 공식문서에도 나와 있듯이, 중앙에서 HTTP 요청을 처리해주는데 이는 Front Controller 패턴으로 설계되어있다.

- DispatcherServlet을 위해 Servlet에 대한 이해를 먼저 해 보자!

##### Servlet
- 자바를 사용하여 웹 페이지를 동적으로 생성하는 서버측 프로그램 혹은 그 사양을 말한다.
  ![image](https://github.com/user-attachments/assets/72432b45-5c90-4570-bf65-532b5327b7e8)
- 위 그림은 HTTP 요청이 들어왔을 때 서버의 서블릿이 어떻게 동작되는지를 나타낸 그림이다.
  - 사용자가 HTTP 요청을 보내면, 요청을 받은 서블릿컨테이너는 HttpServletRequest, HttpServletResponse 각각 요청과 응답을 처리하기 위한 객체를 생성한다.
  - 설정된 정보를 통해서 어떠한 요청인지를 찾고, 해당 요청에 대한 메서드를 호출한 뒤 요청에 따라 doGet, doPost 등의 메서드를 호출한다.
  - 그 결과를 그대로 반환하거나 동적 페이지를 생성한 뒤 응답 객체에 담아서 사용자에게 반환하고, 응답이 완료되면 생성했던 응답과 요청을 위한 객체를 삭제한다.
- 요기까지가 Servlet에 대한 설명이다!
#### 다시 돌아와서
- 이러한 동작 과정을 DispatcherServlet을 사용하여 Front Controller 패턴 방식으로 효율적으로 처리한다.
![image](https://github.com/user-attachments/assets/413f797e-7eec-47e2-bc6a-4e303c64f06d)
- 이렇게 동작한다.
  - 클라이언트에서 요청이 발생하면 DispatcherServlet 객체가 요청을 분석한다. 분석한 데이터를 토대로 Handler Mapping을 통해
  관련된 Controller를 찾아 요청을 전달해준다. Handler Mapping을 통해 API path와 Controller의 메서드를 매칭한다.
  - Controller가 요청에 대한 처리를 완료하고, 결과(데이터와 View 정보)를 전달한다.
  - Dispathcer Servlet 객체는 ViewResolver를 통해 View에 데이터(Model)를 적용하여 View를 클라이언트에게 응답으로 전달한다.

- 요청을 처리할 컨트롤러를 찾아서 위임하고 처리된 결과를 받는 것이라고 생각할 수 있을 것 같다!
출처: https://mozzi-devlog.tistory.com/8 [우당탕탕:티스토리]
