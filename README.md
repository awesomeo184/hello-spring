# 스프링 입문

## 1강 프로젝트 설정

* 스프링 부트로 프로젝트를 만든다( https://start.spring.io/)

- Maven? Gradle?
  - 필요한 라이브러리를 땡겨오고 생명주기를 관리해주는 툴. 최근에는 Gradle을 많이 쓰는 편
- Dependencies(어떤 라이브러리를 땡겨올 것이냐)
  - Spring Web, Thymeleaf(템플릿 엔진)
- build.gradle 파일로 프로젝트 시작
  - build.gradle 파일을 확인해보면 각종 환경설정이 미리 세팅되어 있다. (스프링부트가 알아서 설정해준 것, 버전설정 + 라이브러리 임포트)
  - ` repositories {mavenCentral} `: 메이븐 센트럴이라는 사이트에서 필요한 라이브러리를 다운로드 받음.
  - `dependencies`: 필요한 라이브러리
- Application
  - `@SpringBootApplication`: 메인 함수를 실행하면 내장 톰캣 서버를 띄우면서 프로젝트를 돌림

## 2강 라이브러리 살펴보기

- gradle은 의존 관계가 있는 라이브러리들을 땡겨온다.

  나는 Spring Web과 Thymeleaf만 선택했는데 이와 연관된 라이브러리들이 모두 설치됨.

   Gradle은 의존관계가 있는 라이브러리를 함께 다운로드 한다.

- **스프링 부트 라이브러리**

  - spring-boot-starter-web spring-boot-starter-tomcat: 톰캣 (웹서버) spring-webmvc: 스프링 웹 MVC

  - spring-boot-starter-thymeleaf: 타임리프 템플릿 엔진(View) spring-boot-starter(공통): 스프링 부트 + 스프링 코어 + 로깅

  - spring-boot spring-core

  - spring-boot-starter-logging logback, slf4j

- **테스트 라이브러리**
  - spring-boot-starter-test
  -  junit: 테스트 프레임워크
  -  mockito: 목 라이브러리
  -  assertj: 테스트 코드를 좀 더 편하게 작성하게 도와주는 라이브러리 spring-test: 스프링 통합 테스트 지원
  

## 3강 View 환경설정

### Welcome Page

- src/main/resources/static 에 index.html 파일을 생성하면 스프링부트가 알아서 홈에 렌더링해줌

### Controller



`@Controller`

`@GetMapping("url endpoint")`

`model.addAttribute(key, value)`

key = 템플릿에서 사용할 변수, value = 전달할 값

`return "html이름"`



컨트롤러에서 리턴 값으로 문자열을 반환하면 viewResolver가 화면을 찾아서 처리한다.

`resources:templates/ +{ViewName}+ .html`



pkg hello.hellospring.controller

```java
 @Controller
 public class HelloController {
   
   @GetMapping("hello")
   public String hello(Model model) {
     model.addAttribute("data", "HelloWorld!!"); 
     return "hello";
   }
 }
```



resources/templates/hello.html

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org"> <head>
  <title>Hello</title>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /> </head>
<body>
<p th:text="'안녕하세요. ' + ${data}" >안녕하세요. 손님</p>
</body>
</html>
```



## 4강 빌드하고 실행하기

콘솔로 이동

1. `./gradlew build`

2. `cd build/libs`
3. `java -jar hello-spring-0.0.1-SNAPSHOT.jar`
4. 실행확인



## 5 ~ 7 강 스프링 웹 개발의 기초

1. 정적 컨텐츠
2. MVC와 템플릿 엔진
3. API



### 정적 컨텐츠

스프링 부트는 기본적으로 정적 컨텐츠 기능을 제공한다.

`resources/static/hello-static.html`

```html
<!DOCTYPE HTML>
<html>
<head>
  <title>static content</title>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
정적 컨텐츠 입니다.
</body>
</html>
```

그냥 이렇게 파일을 생성한 뒤 서버를 켜서 `http://localhost:8080/hello-static.html` 로 이동하면 해당 파일이 렌더링된 것을 확인할 수 있음

스프링 부트의 정적 컨텐츠 제공 기능

1. 웹 서버로 요청이 들어오면 톰캣 서버는 해당 내용을 스프링으로 보낸다.

2. 스프링은 컨트롤러에 관련 내용이 있는지 뒤져본다.

3. 없다면 static 디렉토리를 뒤져보고 찾으면 반환한다.



### MVC와 템플릿 엔진

MVC(Model, View, Controller): 관심사의 분리. View는 화면을 그리는데 집중, Model과 Controller는 비즈니스 로직과 데이터 처리에 집중.

```java
    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model) {
        model.addAttribute("name", name);
        return "hello-template";
    }
```



`resources/templates/hello-template.html`

```html
<html xmlns:th="http://www.thymeleaf.org"> <body>
<p th:text="'hello ' + ${name}">여기는 서버 구동 없이 템플릿 파일을 그대로 열었을 때 나타남</p> </body>
</html>
```



`http://localhost:8080/hello-mvc?name=spring` 으로 요청이 들어오면, "spring"을 인자로 받아서 모델의 value로 전달한다.

controller가 model과 return값을 viewResolver에게 전달하면 리졸버는 맞는 화면을 찾아서 처리한다.



### API

view 없이 정보를 바로 전달한다.



Api 방식을 이용할 때는 @ResponseBody 애노테이션을 달아줘야 한다. 그래야 ViewResolver를 사용하지 않고 HTTP BODY에 내용을 직접 전달한다.



```java
    @GetMapping("hello-string")
    @ResponseBody
    public String helloString(@RequestParam("name") String name) {
        return "hello " + name;
    }
```

```java
    @GetMapping("hello-api")
    @ResponseBody
    public Hello helloApi(@RequestParam("name") String name) {
        Hello hello = new Hello();
        hello.setName(name);
        return hello;
    }
```

둘 다 api 방식이지만 첫번째 코드는 문자열을 반환하고 있고, 두번째 코드는 Hello라는 객체를 반환하고 있다.

@ResponseBody 를 사용하면 HTTP의 BODY에 문자 내용을 직접 반환하고 viewResolver 대신에 HttpMessageConverter 가 동작한다.

기본 문자처리는 StringHttpMessageConverter가 기본 객체처리는 MappingJackson2HttpMessageConverter가 담당한다. 객체는 Json 방식으로 넘겨준다.



## 8강 ~ 11강 회원관리 예제, 테스트 케이스 작성

비즈니스 요구사항 정리

컨트롤러

서비스: 핵심 비즈니스 로직 구현 (회원은 중복 가입이 안된다 등)

도메인 : 비즈니스 도메인 객체

리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리 



테스트 케이스는 순서를 보장하지 않는다.

테스트 간에 의존성이 발생하도록 설계해서는 안됨.

테스트 클래스 안에서 생성한 MemoryMemberRepository는 각 테스트 메서드를 수행할 때마다, 내부 값이 변경된다. 

@AfterEach 를 통해서 각 테스트를 실행하고 나서 MemoryMemberRepository를 초기화하는 작업을 해주어야  함.



Actual? expected?

actual에는 테스트하고자 하는 기능의 반환값을 넣어준다. expected에는 그 값이 가져야할 값을 넣어준다.



11강 회원 서비스 작성

비즈니스 로직은 service에 작성 (중복회원 방지 등)



12 회원 서비스 테스트

given, when, then



예외가 제대로 터지는지를 테스트할때는 try catch를 사용할 수도 있지만 assertThrows가 선호된다.

assertThrows(터져야될 예외.class, () -> 이 로직이 실행이 되면)

assertThrows는 예외 객체를 반환한다.



비즈니스 로직에서 사용하는 MemoryMemberRepository 객체를 테스트 로직에서도 공유하기 위해서, MemberService의 생성자로 MemoryMemberRepository를 넣어준다.

@BeforeEach를 통해서 테스트 메서드 하나를 수행할 때마다 멤버 서비스 객체를 독립적으로 생성해준다.

멤버 서비스 입장에서 메모리멤버리포지토리를 주입 당하기 때문에 이런것을 의존성 주입(Dependency Injection)이라고 함



