# ✅ 스프링 프로젝트 환경 설정 정리

## 프로젝트 생성

- [start.spring.io](https://start.spring.io) 에서 Gradle 기반 Spring Boot 프로젝트 생성
- `spring-boot-starter-web`, `thymeleaf` 선택
- `build.gradle` 직접 연동
- Java 17 기반 설정

## 라이브러리 구조 이해

- `spring-boot-starter-web` → 톰캣, MVC 등 포함
- `spring-boot-starter-thymeleaf` → 템플릿 엔진
- `spring-boot-starter-logging` → SLF4J, Logback
- `spring-boot-starter-test` → JUnit5, Mockito 등 포함

## View 설정

- `index.html` → Welcome Page 역할
- Thymeleaf 동작 구조 정리
- `HelloController` + `hello.html`로 테스트

## 빌드 및 실행

- `gradlew build`로 빌드
- `java -jar`로 실행
- 실행 후 [http://localhost:8080](http://localhost:8080)에서 확인 가능
