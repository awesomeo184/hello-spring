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

