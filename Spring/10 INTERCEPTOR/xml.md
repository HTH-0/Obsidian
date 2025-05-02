
---

# 🧾 `servlet-context.xml`을 활용한 Interceptor 등록 예제

## 📄 1. 인터셉터 클래스 (`MyInterceptor.java`)

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
        return true;
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

## 📄 2. XML 등록 설정 (`servlet-context.xml`)

```xml
<!-- servlet-context.xml -->
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/test/**" />
        <bean class="com.example.interceptor.MyInterceptor" />
    </mvc:interceptor>
</mvc:interceptors>
```

### ✅ 설명

- `<mvc:interceptors>` : 스프링 MVC에서 제공하는 인터셉터 등록 태그
    
- `<mvc:interceptor>` : 하나의 인터셉터 정의
    
- `<mvc:mapping path="/test/**" />` : 적용할 URL 패턴
    
- `<bean class="...">` : 실제 인터셉터 클래스 지정
    

---

## 🧭 흐름 요약

XML로 설정하면 DispatcherServlet 초기화 시 등록된 인터셉터가 자동으로 작동함.

```
요청: /test/hello
 → MyInterceptor.preHandle()
   → 컨트롤러 실행
     → MyInterceptor.postHandle()
       → 뷰 렌더링
         → MyInterceptor.afterCompletion()
```

---

## 📌 요약 비교

|구분|Java Config 방식|XML 방식|
|---|---|---|
|설정 위치|`WebMvcConfig.java`|`servlet-context.xml`|
|등록 방법|`registry.addInterceptor()`|`<mvc:interceptor>`|
|유연성|Java 코드 기반, 조건 분기 가능|XML로 명시적이고 간단|
|Spring 버전|Spring 3.1 이상 (현재 권장)|Spring 5 이하에서 많이 사용|

---
