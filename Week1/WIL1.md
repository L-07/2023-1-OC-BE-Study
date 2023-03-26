# Week1 WIL

## 프로젝트 환경설정

### 프로젝트 생성
> **https://start.spring.io**에서 스프링 부트 기반의 프로젝트를 간편하게 생성할 수 있다.  
이때 프로젝트에서 Maven과 Gradle이 있다. 과거에는 자동으로 라이브러리를 관리해주고 프로젝트의 빌드 순서인 라이프사이클까지 관리해주는 Maven을 많이 사용하였다. 하지만 최근에는 스크리트의 길이와 가독성, 실행 속도가 우세한 Gradle을 사용하는 추세이다.  
>
> 프로젝트에서 Group명은 보통 기업명 혹은 기업의 도메인을 작성하고 Artifact는 빌드의 결과물로 보통 프로젝트명이다. Dependencies는 어떠한 라이브러리를 끌어와 사용할 것인지 설정하는 것이다.  
>
> 프로젝트 내의 src 파일에 main과 test가 있다. main의 java 파일 하위에는 실제 패키지와 소스파일 존재하고 main의 resources에는 java 코드 파일을 제외한 xml이나 properties 등 설정파일이나 html이 존재한다. test에는 테스트 코드가 존재해 테스트를 돌려볼 수 있다. build.gradle은 프로젝트를 생성할 때 설정한 것을 확인할 수 있는 파일로 중요하다.

### 라이브러리 살펴보기
> build.gradle 파일을 확인해보면 프로젝트 생성 시 추가한 Spring Web, Thymeleaf와 test 이렇게 세가지만 보이지만, External Libraries 폴더를 확인해보면 웹서버에서 내장되어 온 tomcat, spring-boot 등 굉장히 많은 라이브러리들을 확인할 수 있다.이는 **Gradle이 의존관계에 있는 라이브러리를 자동으로 관리해 받아오기 때문**에 발생한 일이다.  
>
> Dependecies 파일을 보면 여러 라이브러리들을 확인할 수 있는데 이떄 *로 표시된 항목들은 중복을 제거해준 것이다.  
>
> 스프링 부트의 주요 라이브러리들로는 **spring-boot-starter-web**, **spring-boot-starter-tomcat**, **spring-webmvc**, **spring-boot-starter-thymeleaf**, **spring-boot-starter** (스프링 부트와 관련된 라이브러리, spring-core, spring-boot-starter-logging)이 있다.  
현업에서는 로그 파일 관리와 오류 관리를 위해 출력을 할 때 System.out.println보다 **Log**를 사용한다. 실제로 어떤 구현체로 출력할지 결정할 때 logback을 사용한다.  
>
> test와 관련된 라이브러리들은 spring-boot-starter-test에 존재한다. **junit**는 테스트를 위해 사용하는 라이브러리이다. mocktio, assertj는 테스트를 실행할 때 편리하게 한다. spring-test는 스프링 통합 테스트를 도와준다.

### View 환경설정
> 프로젝트에서 src/main/static에 html 파일을 만들어 코드를 작성하면 localhost:8080에 구현된다.  
>
> src/main/java/hello.hellospring에 controller라는 패키지를 만들고 그 하위에 파일을 만들고 리턴 값을 호출하는데, 이 리턴은 src/main/resources/templates에 html 파일을 만들면 컨트롤러 하위의 파일에서 키와 데이터 값의 주소를 받아온다.

### 빌드하고 실행하기
> 프로그램을 사용하지 않고 터미널에서도 빌드를 할 수 있다.  
프로젝트 파일에서 gradlew build라는 커맨드를 작성하면 build 폴더가 생기고 그 내부에 libs 폴더가 생성된다.이 내부에 생성된 jar 파일을 java -jar *.jar로 실행하면 서버가 실행된다.


## 스프링 웹 개발 기초
웹 개발 방법으로는 세가지가 있다.  
**정적 컨텐츠**는 파일을 웹 브라우저에 그대로 전달하는 반면 **MVC** (model, view, controller)와 템플릿 엔진은 가장 많이 사용하는 방식으로, 서버에서 변형을 가하여 전달한다. 마지막으로 **API**는 특정한 데이터 포맷인 xml 혹은 JSON (JavaScript Object Notation) 등을 사용하여 전달하는 방식이다.

### 정적 컨텐츠
> 스프링 부트는 static을 찾아 정적 컨텐츠 기능을 자동으로 지원한다.  
>
> localhost:8080/파일명.html로 접속하면 **작성한 소스 코드가 그대로 반환**된다.  
이는 내장 톰캣 서버가 주소에 대한 요청을 받으면 스프링이 우선 컨트롤러를 찾는다. 이때 컨트롤러가 없으면 resources 내부에서 파일을 찾아 반환하는 원리이다.

### MVC와 템플릿 엔진
MVC : Model, View, Controller
> MVC는 용이한 코드 관리를 위해 구성요소를 세가지로 구분한 디자인 패턴이다.  
> ```java
>    @GetMapping("hello-mvc")
>    public String helloMVC(@RequestParam("name") String name, Model model) {
>       model.addAttribute("name", name)
>       return "hello-template";
>    }   
> ```
> 위와 같이 controller에 코드를 작성하고 templates에 html 파일을 생성한 후 http://localhost:8080/hello-mvc?name=인자 로 접속하면 인자가 출력되는 것을 확인할 수 있다.   
>
> 이는 정적 컨텐츠와 유사하게 내장 톰캣 서버가 요청을 받아 hello-mvc를 스프링에 전달을 하면 컨트롤러가 매핑되어 있는 메소드를 호출한다. 이때 키가 name이고 값은 인자인 hello-template를 반환한다. 여기서 스프링의 **viewResolver가 리턴값인 hello-template.html을 찾아 thymeleaf 템플릿 엔진으로 넘겨 템플릿 엔진이 변환을 한 후 전달**한다.

### API
API : Application Programming Interface
> ```java
>    @GetMapping("hello-string")
>    @ResponseBody
>    public String helloString(@RequestParam("name") String name) {
>       return "hello " + name;
>    }
> ```
> ```@ResponseBody```는 http의 body 부분에 데이터를 직접 넣어주겠다는 의미로, 전달한 인자를 출력할 수 있도록 한다.   
>
> http://localhost:8080/hello-string?name=인자 로 접속을 하면 MVC, 템플릿 엔진처럼 출력이 되는데 소스코드를 확인하면 "hello 인자" 가 그대로 작성되어 있는 것을 확인할 수 있다.  
>
> 이는 문자를 전달하는 방식이었는데, 객체를 전달하는 방식은 아래와 같다.
> ```java
>    @GetMapping("hello-api")
>    @ResponseBody
>    public Hello helloAPI(@RequestParam("name") String name) {
>        Hello hello = new Hello();
>        hello.setName(name);
>        return hello;
>    }
>
>    static class Hello {
>        private String name;
>
>        public String getName() {
>            return name;
>        }
>
>        public void setName(String name) {
>            this.name = name;
>        }
>    }
> ```
> http://localhost:8080/hello-api?name=인자 에 접속할 경우 출력 결과와 소스코드가 모두 {"name":"인자"}로 동일하다. 이는 키와 value로 이루어진 JSON 방식이다.  
>
> ```@ResponseBody```는 스프링이 내장 톰캣 서버로부터 hello-api를 전달받아 컨트롤러를 찾는다. 하지만 MVC, 템플릿 엔진과 달리 ResponseBody가 존재하므로 viewResolver가 아닌, **HttpMessageConverter가 동작**한다. **문자일 때는 StringHttpMessageConverter가 동작하여 바로 전달**, **객체일 때는 MappingJackson2HttpMessageConverter가 동작하여 JSON 방식으로 변환 후 전달**한다.

> 여러 프로그램들을 연결하고 통신할 때 애플리케이션의 모든 코드와 데이터를 공유하면 보안 등의 문제가 발생할 수 있다. 이때 **API가 인터페이스를 제공하여 접근 범위와 권한을 제어**하는 것이다.  
>
> 사용자가 정보에 접근하기 위해 요청을 보내는 애플리케이션을 **클라이언트**, 이 요청에 대해 데이터를 받아 응답을 보내는 애플리케이션을 **서버** (API 서버)라고 한다.

> REST (Representational State Transfer)를 기반으로 구현한 REST API가 REST의 설계 원리에 부합할 때 **RESTful**이라고 한다.  
>
> **REST는 자원을 이름으로 구분하여 상태로 전달하는 소프트웨어 아키텍처**이다.  
>
> REST의 원칙은 다음과 같다.
> + **Uniform Interface**
>    + 일관된 인터페이스의 4가지 조건
>       + Identification of resources
>       + Manipulation of resources through representations
>       + Self-descriptive messages
>       + Hypermedia as the engine of application state (HATEOAS)
> + **Stateless**
>    + 클라이언트의 context가 서버에 저장되지 않음
> + **Cacheable**
>    + 캐싱이 가능해 효율성이 높음
> + **Layered System**
>    + 계층화 시스템을 통하여 시스템의 확장성을 높임
> + **Server-Client**
>    + 서버와 클라이언트의 역할을 명확히 구분해 독립적으로 동작
> + Code on demand
>    + 선택적 원칙으로, 서버에서 클라이언트로 코드를 보내서 실행이 가능