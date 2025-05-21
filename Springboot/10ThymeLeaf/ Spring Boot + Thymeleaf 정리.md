
---

#  Spring Boot + Thymeleaf 정리

## 1. 📌 Thymeleaf란?

- HTML을 렌더링하는 Java 기반의 템플릿 엔진
    
- HTML5 문법 기반 → 브라우저에서 열어도 깨지지 않음
    
- JSP보다 직관적이고, 속성 기반 문법이 강력함
    
- Spring MVC와 매우 높은 호환성
    

---

## 2. ⚙️ 의존성 설정

### ✅ Maven

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

### ✅ Gradle

```groovy
implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
```

---

## 3. 📁 디렉토리 구조

```bash
src/
└── main/
    ├── java/
    │   └── com.example.demo/
    │       └── controller/
    │           └── HomeController.java
    └── resources/
        ├── static/         # 정적 리소스(css, js, images 등)
        ├── templates/      # HTML 템플릿 파일들
        └── application.yml or application.properties
```

---

## 4. 🌐 기본 컨트롤러 예제

```java
@Controller
public class HomeController {

    @GetMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("name", "홍길동");
        return "hello"; // templates/hello.html
    }
}
```

---

## 5. 🖼️ Thymeleaf 템플릿 예제

### `templates/hello.html`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Hello Page</title>
</head>
<body>
    <h1>안녕하세요, <span th:text="${name}">이름</span>!</h1>
</body>
</html>
```

---

## 6. 🔤 주요 문법 정리

### ✅ 변수 출력

```html
<p th:text="${message}"></p>
```

### ✅ 조건문

```html
<div th:if="${user != null}">
  <p th:text="${user.name}"></p>
</div>

<div th:unless="${user != null}">
  <p>사용자 정보 없음</p>
</div>
```

### ✅ 반복문

```html
<ul>
  <li th:each="item : ${itemList}" th:text="${item}"></li>
</ul>
```

### ✅ 링크 연결

```html
<a th:href="@{/hello}">Hello 이동</a>
```

### ✅ 입력 폼

```html
<form th:action="@{/form}" method="post">
    <input type="text" name="username" th:value="${username}" />
    <button type="submit">제출</button>
</form>
```

---

## 7. 📝 폼 처리 예제

### ✅ 컨트롤러

```java
@Controller
public class FormController {

    @GetMapping("/form")
    public String showForm(Model model) {
        model.addAttribute("username", "");
        return "form";
    }

    @PostMapping("/form")
    public String submitForm(@RequestParam String username, Model model) {
        model.addAttribute("username", username);
        return "result";
    }
}
```

### ✅ `form.html`

```html
<form th:action="@{/form}" method="post">
    <input type="text" name="username" />
    <button type="submit">제출</button>
</form>
```

### ✅ `result.html`

```html
<p th:text="'입력한 이름은: ' + ${username}"></p>
```

---

## 8. 👤 객체 바인딩 예제

### ✅ DTO

```java
public class User {
    private String name;
    private int age;
    // getter/setter
}
```

### ✅ 컨트롤러

```java
@Controller
public class UserController {

    @GetMapping("/user")
    public String showForm(Model model) {
        model.addAttribute("user", new User());
        return "userForm";
    }

    @PostMapping("/user")
    public String submit(@ModelAttribute User user, Model model) {
        return "userResult";
    }
}
```

### ✅ `userForm.html`

```html
<form th:action="@{/user}" th:object="${user}" method="post">
    이름: <input type="text" th:field="*{name}" /><br/>
    나이: <input type="number" th:field="*{age}" /><br/>
    <button type="submit">전송</button>
</form>
```

### ✅ `userResult.html`

```html
<p th:text="'이름: ' + ${user.name}"></p>
<p th:text="'나이: ' + ${user.age}"></p>
```

---

## 9. 🧱 레이아웃 적용 (thymeleaf-layout-dialect)

### ✅ 의존성 추가

```xml
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
    <version>3.1.0</version>
</dependency>
```

### ✅ `layout.html`

```html
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<head>
    <title layout:title-pattern="$CONTENT_TITLE - My Site">기본 타이틀</title>
</head>
<body>
    <header><h1>공통 헤더</h1></header>

    <section layout:fragment="content">
        본문이 삽입됩니다.
    </section>

    <footer><p>공통 푸터</p></footer>
</body>
</html>
```

### ✅ 실제 페이지 예시

```html
<html xmlns:th="http://www.thymeleaf.org"
      xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
      layout:decorate="~{layout}">
<head>
    <title>사용자 페이지</title>
</head>
<body>
<section layout:fragment="content">
    <h2>사용자 정보</h2>
</section>
</body>
</html>
```

---

## 🔚 정리 요약

|항목|설명|
|---|---|
|템플릿 경로|`resources/templates/`|
|정적 리소스|`resources/static/`|
|주요 태그|`th:text`, `th:each`, `th:if`, `th:href`, `th:object`|
|폼 처리|`@ModelAttribute`, `@RequestParam`|
|객체 바인딩|`th:field`, `th:object`|
|레이아웃 구성|`layout:decorate`, `layout:fragment`|
|장점|동적 HTML 렌더링 + 서버사이드 검증/출력 모두 가능|

---
