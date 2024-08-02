![image](https://github.com/user-attachments/assets/03243d3e-3412-46d3-8be3-ccf9a20f0643)#### Filter와 Spring Security
![image](https://github.com/user-attachments/assets/7965901a-d7e8-4848-a848-0ddbd43ac385)
- Filter: Dispatcher Servlet의 앞단에 위치하여 각각 요청의 최초단계와 응답의 최종 단계에서 부가적인 동작을 수행한다.
- 주로 범용적으로 처리해야 하는 작업에 사용한다.
- 특히 사용하는 것이 인증과 인가에 사용한다.
![image](https://github.com/user-attachments/assets/678d1d82-e465-46a5-abee-30b1216062ed)
- 사진 처럼 여러 필터를 두어 각각을 통과하도록 Chain 형식으로 묶을 수 있다.

#### Interceptor
- Interceptor: Spring이 제공하는 기술로, 필터와 비슷한데, 인터셉터는 컨트롤러를 호출하기 전, 후에요청과 응답을 참조하거나 가공하는 부가 기능을 제공한다.
필터는 웹 컨테이너(서블릿 컨테이너)에서 동작하고인터셉터는 **스프링 컨텍스트**에서 동작한다.
![img1 daumcdn](https://github.com/user-attachments/assets/94e1c15f-4858-4b1f-bcc8-2bd89d13c741)
이렇게 Dispatcher Servlet 다음에 위치한다.

#### Filter vs Interceptor
- Filter는 요청 객체와, 응답 객체를 조작할 수 있다. Interceptor는 안 된다.
인터셉터는 해당 인터셉터에서 true를 반환하면 다음 인터셉터가 실행되거나 컨트롤러로 요청을 전달한다. false라면 요청이 중단된다.
- Filter는 주로 특정 요청과 컨트롤러에 관계없이 전역적으로 처리해야 하는 작업이나 전반적으로 사용되는 기능을 구현할 때 적용한다.
- Interceptor는 요청과 관련된 작업에 대해 추가적인 요구사항을 처리할 때 적용한다.  

#### Filter in Spring Security
- Spring Security는 본디 인증과 인가를 제공하는 데에 초점을 맞춘 프레임워크이다! 그리고 서블릿 필터를 기반으로 하고 있기 때문에
Spring Security가 이루는 Filter에 대해서 알아보자!
![image](https://github.com/user-attachments/assets/5b26a0eb-770f-4cf3-b129-ee6faf0d0289)

- Spring Security는 서블릿 컨테이너의 라이프 사이클과 스프링의 애플리케이션 컨텍스트 사이를
연결하는 DelegatingFilterProxy라는 Filter의 구현체를 제공한다. 왜료??? 바로 필터를 구현하면 Spring Bean을 인식하지 못한다. 그렇기 때문에 Spring에 구현을 위임한다.
  - DelegatingFilterProxy는 GenericFilterBean이라는 클래스를 상속하는데, GenericFilterBean 클래스는 Filter 인터페이스를 구현하는 구현체이다.
   - DelegatingFilterProxy는 Filter 인터페이스의 구현체를 스프링이 관리하는 빈에 위임한다.
   - 실제 보안처리를 하지는 않는다! 서블릿 컨테이너와 스프링을 연결해주는 역할을 한다!
- 그렇다면 여기서 드는 의문. 그러면 DelegatingFilterProxy도 스프링 컨테이너에 접근을 해야 되는데, 어떻게 접근하는 걸까??

**바로!**  SecurityFilterAutoConfiguration에서 DelegatingFilterProxyRegistrationBean이 빈으로 등록된다. -> DelegatingFilterProxyRegistrationBean의 
getFilter()를 통해서 DelegatingFilterProxy의 인스턴스가 생성이 된다. -> DelegatingFilterProxy가 컨테이너에서 필터체인을 찾는다.

- 조금 더 자세히 말하자면, springSecurityFilterChain이라는 이름이 존재하는 경우 DelegatingFilterProxyRegistrationBean을 빈으로 등록한다. 이후 getFilter() 메서드를 통해
DelegatingFilterProxy 인스턴스를 반환하고, springSecurityFilterChain이라는 이름의 빈을 찾아 요청 처리를 위임한다!
- 결국에는 DelegatingFilterProxy 자체가 빈으로 등록되기 보다는 DelegatingFilterProxyRegistrationBean이 빈으로 등록되면서 컨테이너에 접근할 수 있는 것이다.

##### FilterChainProxy
- 그렇다면 springSecurityFilterChain이라는 이름의 빈은 뭘까?
  - "WebSecurity 는 WebSecurityConfiguration 에 의해 생성되어 Spring Security Filter Chain(springSecurityFilterChain)으로 알려진 FilterChainProxy 생성합니다." 라고 WebSecurity 클래스 설명에 나와있다. 그렇다는건?
springSecurityFilterChain은 FilterChainProxy라는 것이다! 사실 springSecurityFilterChain == FilterChainProxy라기 보다는 여러 필터들이 연결된 SecurityFilterChain을 관리하는 역할을 한다고 볼 수 있다. 
-  DelegatingFilterProxy가 FilterChainProxy에게 요청 처리를 위임하는 것이다! FilterChainProxy는 각 요청에 맞는 SecurityFilterChain을 찾아 필터를 실행한다!
-  ##### 너무 어렵다. Filter에서 시작해서 Spring Security가 어떤 역할을 하고, 내부에서 그 많은 Filter들을 어떻게 관리하는지까지 찾아 보게 될 줄은 몰랐다....
-  ##### 다음에는 여러 Filter에 대해서 알아봐야겠다!
