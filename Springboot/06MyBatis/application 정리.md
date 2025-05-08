
---

# ✅ SpringBoot + MyBatis + HikariCP + JSP 연동 통합 구조 정리

---

## 1️⃣ application.properties 설정

### 📂 `application.properties`

```properties
spring.application.name=demo

# 서버 포트 설정
server.port=8090

# UTF-8 인코딩 필터 설정
spring.servlet.filter.encoding.filter-name=encodingFilter
spring.servlet.filter.encoding.filter-class=org.springframework.web.filter.CharacterEncodingFilter
spring.servlet.filter.encoding.init-param.encoding=UTF-8
spring.servlet.filter.encoding.init-param.forceEncoding=true
spring.servlet.filter.encoding.url-pattern=/*

# JSP 설정
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
server.servlet.jsp.init-parameters.development=true

### DataSource 설정은 수동 Java Config 사용 중이므로 주석 처리됨
#spring.datasource.url=jdbc:mysql://localhost:3306/testdb
#spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#spring.datasource.username=root
#spring.datasource.password=1234
```

---

## 2️⃣ 주요 설정 흐름 요약

|항목|설명|
|---|---|
|포트 번호|`8090`으로 설정됨|
|인코딩|UTF-8 강제 적용 (`CharacterEncodingFilter`)|
|JSP 뷰 설정|`/WEB-INF/views/` + `.jsp`|
|DataSource|JavaConfig(`DataSourceConfig.java`)에서 HikariCP 수동 등록 사용 중|
|MyBatis|`MybatisConfig.java`에서 SqlSessionFactory 수동 등록 및 XML 매퍼 적용|

---

## 3️⃣ JSP 연동 예시

**예: `/memo/list` 요청이 들어오면 → `/WEB-INF/views/memo/list.jsp` 로 이동**

### 📂 컨트롤러 예시

```java
@Controller
@RequestMapping("/memo")
public class MemoController {

    @Autowired
    private MemoMapper memoMapper;

    @GetMapping("/list")
    public String list(Model model) {
        List<MemoDto> list = memoMapper.selectAll();
        model.addAttribute("list", list);
        return "memo/list"; // => /WEB-INF/views/memo/list.jsp
    }
}
```

### 📂 JSP 예시 (`/WEB-INF/views/memo/list.jsp`)

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head>
    <title>Memo List</title>
</head>
<body>
<h1>메모 목록</h1>
<c:forEach var="memo" items="${list}">
    <p>${memo.id} - ${memo.text} - ${memo.writer}</p>
</c:forEach>
</body>
</html>
```

---

## ✅ 전체 요약

|구성 요소|상태|
|---|---|
|포트 설정|`8090`|
|인코딩 설정|UTF-8 필터 적용|
|JSP 뷰 해석기|`/WEB-INF/views/ + .jsp`|
|DB 연결|`DataSourceConfig.java` → HikariCP 사용|
|MyBatis 설정|`MybatisConfig.java` + Mapper 인터페이스 + XML|
|테스트 코드|`MemoMapperTest`, `DataSourceConfigTest`로 JDBC, MyBatis 각각 확인|
|DTO 유효성 검사|`@Min`, `@Max`, `@NotBlank`, `@Email` 등 포함|
|Mapper 방식|어노테이션 & XML 혼용 방식|

---
