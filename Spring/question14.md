#### Spring Bean Life Cycle에 대해 설명해주세요
- 스프링 IOC 컨테이너에 등록된 빈 객체의 생명주기를 말하는 것 같다.

#### 자세히 들어가 봅시다잇
- 스프링 컨테이너에 등록되어 스프링 컨테이너가 관리하는 빈은 다음의 생명 주기를 가진다.
1. 빈 생성 -> 2. 의존성 주입 -> 3. 초기화 콜백 ->  4. 사용 -> 5. 소멸 전 콜백 -> 6. 빈 소멸
##### 빈 생명주기 콜백
- 빈 생성: 빈 등록과 혼동될 수 있는데, 밀접하게 관련되어 있으나 구분되는 개념이다. 빈이 생성된다는 것은 우리가 코드에서 @Component, @Bean 처럼 빈으로 사용하기 위해 작성한 클래스, POJO 객체들이 각각의 객체에 지정된 Scope에 따라서 해당 POJO의 인스턴스가 생성된다는 것을 말하고, 빈 등록은 생성된 인스턴스가 스프링 컨테이너에 등록되어 관리된다는 것을 말한다.

- 의존성 주입: 이렇게 생성되고 등록된 빈을 사용하기 위해서는 의존성 주입을 해주어야 한다. 생성자 주입, 수성자 주입 등 다양한 방법을 통해 의존관계를 주입할 수 있다. 이렇게 의존관계 주입까지 완료되면 해당 빈을 사용할 준비가 됐다는 것이다.

- 초기화 콜백: 의존관계 주입까지 완료되면 초기화 콜백이 호출된다. 스프링은 여러가지 초기화 콜백 방법을 지원한다.
초기화란 빈을 사용할 수 있는 상태로 만드는 작업을 말한다. 생성과 의존관계 주입을 마친 후에 추가적인 설정이나 준비 작업을 진행하는 것이다. 
스프링은 초기화를 다양한 방법을 통해 지원한다.
1. InitializingBean 인터페이스: 스프링이 지원해주는 방법으로, afterPropertiesSet() 메서드를 구현하여 초기화 로직을 작성할 수 있다. (진짜 오래된 방법이다. 2003년...?)

2. @PostConstruct 어노테이션: 초기화 메서드에 @PostConstruct 어노테이션을 붙여 초기화 로직을 작성한다. PostConstruct은 JSR-250 스펙이기 때문에 JSR-250을 구현한 다른 프레임워크 혹은 라이브러리를 사용해 된다. 좀 더 Spring에 의존적이지 않다는 장점이 있다.
3. XML 설정: XML 설정 파일에서 init-method 속성을 사용하여 초기화 메서드를 지정한다.
4. @Bean: @Bean에는 initMethod()가 있어 이를 이용하여 초기화를 할 수 있다.

호출순서는 @PostConstruct -> afterPropertiesSet() -> initMethod() 순서이다.

- 빈 사용: 비즈니스 로직에서 열심히 사용하면 된다.

- 소멸 전 콜백: 빈이 사용한 리소스를 반환하거나, 종료 시점에 처리해야 할 작업이 있을 시 소멸 전에 콜백을 해야 한다. 다양한 방법이 있다!

1. DisposableBean: 스프링이 지원해주는 방법이다.  DisposableBean 인터페이스도 추상메서드 destroy()만 구현해주면 Spring이 알아서 호출해 준다.
2. @PreDestroy: @PostConstruct와 짝꿍이다. JSR-250 스펙에 따라 구현되어 있고, 종료 시점에 어노테이션을 작성하면 된다.
3. @Bean: @Bean 어노테이션에 destroyMethod()가 있다. 이를 이용하여 종료 콜백 메서드를 작성하면 된다.
4. XML: 설정에서 destroy-method를 사용하면 된다!

#### 레전드 TMI
- LifeCycle 인터페이스라고 있다. 스스로의 생명주기 요구사항을 가진 객체들을 위한 메서드를 정의한 인터페이스이다.

- 스프링의 관리를 받는 모든 객체들은 LifeCycle 인터페이스를 구현하고 있다. ApplicationContext가 시작과 정지 신호를 받으면 context 내 정의된 모든 Lifecycle 구현체에 그 신호를 전달한다. 다음에서 보여주는 LifecycleProcessor에게 위임함으로써 그 일을 해낸다.
- LifecycleProcessor는 LifeCycle 인터페이스를 상속하는 인터페이스이다. 이를 통해 context가 새로고침 되고 닫히는 것에 반응할 수 있다.
TMI니까 나중에 다시 작성...너무 어렵다.
