
---

# 🛡️ Spring MVC Interceptor 구현 예제 정리

## 📦 전체 코드

### ✅ 1. `MyInterceptor.java`

```java
package com.example.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

public class MyInterceptor extends HandlerInterceptorAdapter {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        System.out.println("[인터셉터] preHandle 실행");
        return true; // false 반환 시 컨트롤러로 진행되지 않음
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            org.springframework.web.servlet.ModelAndView modelAndView) throws Exception {
        System.out.println("[인터셉터] postHandle 실행");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
            Exception ex) throws Exception {
        System.out.println("[인터셉터] afterCompletion 실행");
    }
}
```

---

### ✅ 2. `WebMvcConfig.java` 내 인터셉터 등록 추가

```java
package com.example.app.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.example.interceptor.MyInterceptor;

@Configuration
@EnableWebMvc
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // "/test/**" 경로로 들어오는 요청에만 적용
        registry.addInterceptor(new MyInterceptor())
                .addPathPatterns("/test/**");
    }
}
```

---

### ✅ 3. 테스트용 컨트롤러 (`TestController.java`)

```java
package com.example.app.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class TestController {

    @GetMapping("/test/hello")
    public String testPage() {
        System.out.println("[컨트롤러] /test/hello 요청 처리");
        return "test/hello";
    }
}
```

---

### ✅ 4. JSP 뷰 예제 (`/WEB-INF/views/test/hello.jsp`)

```jsp
<%@ page contentType="text/html;charset=UTF-8" %>
<html>
<head><title>인터셉터 테스트</title></head>
<body>
<h2>인터셉터 테스트 페이지</h2>
</body>
</html>
```

---

## 🔍 코드 설명

### ✅ `MyInterceptor.java`

- `preHandle()` : 컨트롤러 실행 전에 호출됨. 인증/로깅 등 주요 제어 지점
    
- `postHandle()` : 컨트롤러가 정상 실행된 후, 뷰 렌더링 전에 호출
    
- `afterCompletion()` : 뷰까지 렌더링 완료된 후 호출. 리소스 정리 용도
    

### ✅ `WebMvcConfig.java`

- `addInterceptors()` 메서드를 통해 인터셉터 등록
    
- `.addPathPatterns("/test/**")` : `/test/` 하위 경로에만 적용
    
- 이 방식은 `servlet-context.xml` 대신 Java Config 기반 Spring 설정 방식
    

### ✅ `TestController.java`

- `/test/hello` 요청을 처리하는 단순 컨트롤러
    
- 이 URL이 인터셉터 대상이기 때문에 `MyInterceptor`의 모든 메서드가 실행됨
    

---

## 🧭 실행 흐름 요약

```
클라이언트 요청 (/test/hello)
 → MyInterceptor.preHandle()
   → TestController 실행
     → MyInterceptor.postHandle()
       → 뷰 렌더링 (hello.jsp)
         → MyInterceptor.afterCompletion()
```

---

## 📌 요약

|항목|설명|
|---|---|
|핵심 클래스|`HandlerInterceptorAdapter` (Spring 5 이하 권장)|
|등록 위치|Java Config (`WebMvcConfig`) 또는 XML|
|동작 지점|preHandle → postHandle → afterCompletion 순|
|용도|인증/권한, 로깅, 응답 처리, 리소스 정리 등|
|테스트 경로|`/test/**` 하위에서만 인터셉터 작동|

---
