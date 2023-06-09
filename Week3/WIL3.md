# Week3 WIL

의존과 의존성
> 1주차에서 라이브러리를 확인할 때 Gradle이 추가하지 않았지만 의존관계에 있는 라이브러리들을 자동으로 관리해 받아온다고 하였다. 또한 Dependencies 파일에서 중복이 존재한다는 것도 알 수 있었다. 이는 객체 간의 의존성이 존재한다는 것으로, 바인딩과 유사하다.  
>   
> 의존성이란 파라미터나 리턴값 또는 지역변수 등으로 다른 객체를 참조하는 것을 의미한다.  
> 이때 클래스 간의 결합성이 높아 유연성이 떨어진다는 문제점과, 객체가 아닌 클래스 간의 관계가 맺어진다는 문제가 생긴다.
> 이를 해결하기 위해 의존관계 주입(Dependency Injection, DI) 디자인 패턴을 사용한다.

@Autowired 의존성 주입
> DI는 의존 관계를 외부에서 결하는 것으로 클래스가 아닌 인터페이스를 생성하여 추상화하는 것이다.
> 이를 통해 코드의 재사용성이 높아지고, 코드 작성이 유연해지며, 테스트가 용이해진다.  
> 
> 이러한 방법으로 생성자(Constructor) 주입, 필드(Field) 주입, 수정자(Setter) 주입 세가지의 방법이 있는데, 이때 @Autowired를 이용하여 DI를 한다.  
>
> 생성자 주입은 단일 생성자의 경우 @Autowired가 생략되어도 되지만 생성자가 두개 이상일 경우 붙여줘야 한다. 또한 다른 DI 방식과 달리 필드에 final 선언을 할 수 있다.  
> 필드 주입은 필드에 @Autowired를 붙여 의존성을 주입하는 방식이다.  
> 수정자 주입은 Setter 메소드에 @Autowired를 붙여 의존성을 주입한다.

DIP
> 객체지향의 5대 원칙(SOLID)는 다음과 같다.
> > 단일 책임 원칙(Single Responsibility Principle SRP)  
> > 개방 폐쇄 원칙(Open Closed Principle, OCP)  
> > 리스코프 치환 원칙(Liscov Substitution Principle, LSP)  
> > 인터페이스 분리 원칙(Interface Sergregation Principle, ISP)  
> > 의존성 역전 원칙(Dependency Inversion Principle, DIP)
> 
> 이중 DIP는 의존성 역전 원칙으로 전술한 의존성의 문제와 직접적으로 연관이 있는, 구체화가 아닌 추상화에 의존해야한다는 원칙이다.  
> DIP는 고수준 모듈인 인터페이스(추상 클래스)를 저수준 모듈인 객체(메인 클래스)의 구현의 의존하는 것이 아닌, 객체가 인터페이스에서 정의한 추상 타입에 의존한다는 것이다.

스프링 빈과 스프링 컨테이너란?
> 스프링 빈이란 스프링 컨테이너가 관리하는 자바 객체이고, 이때 스프링 컨테이너는 스프링에서 자바 객체들을 관리하는 공간이다.  
> 스프링의 특징으로는 제어의 역전(Inversion Of Control. IoC)이 있는데 프레임워크를 사용하여 객체의 생명 주기를 모두 프레임워크에 위임해 외부 라이브러리가 프로그래머가 작성한 코드를 호출하고 흐름을 제어하는 것이다. 이때 스프링에 의하여 관리당하는 자바 객체가 빈이다.

JDBC와 JPA?
> JDBC(Java DataBase Connectivity)는 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API이고, JPA(Java Persistence API)란 자바 진영 ORM 기술 표준으로, 관계형 데이터베이스의 관리를 표현하는 자바 API이다.