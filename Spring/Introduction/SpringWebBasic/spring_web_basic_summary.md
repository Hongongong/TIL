# 🌐 스프링 웹 개발 기초 정리

<br>

## 1. 정적 컨텐츠

- **위치**: `resources/static`
- **특징**
  - Controller 없이도 브라우저 요청에 응답
  - 파일을 **가공 없이 그대로 반환**
  - 빠르고 간단함 (HTML, CSS, JS, 이미지 등)

### ✅ 동작 순서
1. 요청이 들어오면 내장 톰캣이 URL을 수신
2. 관련 Controller가 없으면 `static/` 경로에서 파일 탐색
3. 해당 파일이 있으면 그대로 반환

![정적 컨텐츠 흐름](images/image%201.png)

<br>

## 2. MVC + 템플릿 엔진 (예: Thymeleaf)

- **위치**: `resources/templates`
- **특징**
  - `Controller → Model → ViewResolver → HTML 렌더링`
  - 동적 데이터를 HTML에 삽입 (템플릿 엔진 사용)
  - 관심사 분리로 유지보수 용이

### ✅ 예시

```java
@GetMapping("hello-mvc")
public String helloMvc(@RequestParam("name") String name, Model model) {
    model.addAttribute("name", name);
    return "hello-template"; // templates/hello-template.html 로 연결됨
}
```

```html
<p th:text="'hello ' + ${name}">hello! empty</p>
```

> URL 예시: `http://localhost:8080/hello-mvc?name=spring`

![MVC 흐름](images/image%203.png)

<br>

### 📊 정적 컨텐츠 vs MVC + 템플릿 엔진

| 구분 | 정적 컨텐츠 | MVC + 템플릿 |
|------|-------------|---------------|
| 경로 | `/static` | `/templates` |
| 반환 방식 | 가공 없이 반환 | HTML로 렌더링 후 반환 |
| 동적 처리 | ❌ 없음 | ✅ 있음 (Model 활용) |
| Controller 필요 | ❌ 없음 | ✅ 필요 |

<br>

## 3. API (문자 or 객체)

### 3-1. @ResponseBody (문자 반환)

- HTML 태그 없이 순수 텍스트 반환
- ViewResolver를 거치지 않음 → HttpMessageConverter 사용

```java
@GetMapping("hello-string")
@ResponseBody
public String helloString(@RequestParam("name") String name) {
    return "hello " + name;
}
```

> 결과: `hello zz`

---

### 3-2. @ResponseBody (객체 반환 → JSON)

- 객체를 반환하면 JSON으로 자동 변환됨 (Jackson 사용)

```java
@GetMapping("hello-api")
@ResponseBody
public Hello helloApi(@RequestParam("name") String name) {
    Hello hello = new Hello();
    hello.setName(name);
    return hello;
}

static class Hello {
    private String name;
    // getter/setter 생략
}
```

> 결과: `{"name":"spring2"}`

![API 흐름](images/image%207.png)



## 🔄 @ResponseBody 작동 원리 요약

| 반환 타입 | 동작 | 사용되는 Converter |
|-----------|------|--------------------|
| `String` | 텍스트 그대로 반환 | `StringHttpMessageConverter` |
| 객체 (ex. DTO) | JSON으로 변환 | `MappingJackson2HttpMessageConverter` |

> (참고) 컨버터 선택 기준: `클라이언트의 Accept 헤더 + 반환 타입`

---

## ✅ 정리

| 방식 | 위치 | 변환 여부 | 특징 |
|------|------|-----------|-------|
| 정적 컨텐츠 | `/static` | ❌ | HTML, CSS, JS 등 정적 파일 가공없이 그대로 반환 |
| MVC + 템플릿 엔진 | `/templates` | ✅ | Controller → Model → ViewResolver → HTML 렌더링 |
| API | Controller 내부 | ✅ (문자 / 객체) | ViewResolver를 거치지 않음 → HttpMessageConverter 사용 |
