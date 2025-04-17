# 🧪 서블릿 리스너 테스트 서블릿 정리

---

## 📄 C02ListenerTest.java

```java
package Servlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(urlPatterns = {"/app/add","/app/replace","/app/remove"})
public class C02ListenerTest extends HttpServlet{

	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("[SERVLET] C02ListenerTest service");
		String uri = req.getRequestURI();
		if(uri.contains("/app/add")) {
			req.getServletContext().setAttribute("CTX_KEY", "CTX_VALUE");
			
		}else if(uri.contains("/app/replace")) {
			req.getServletContext().setAttribute("CTX_KEY", "CTX_VALUE_2");
		}else {
			req.getServletContext().removeAttribute("CTX_KEY");
		}
	}
}
```

---

## 📄 C03ListenerTest.java

```java
package Servlet;

public class C03ListenerTest {

}
```

> ⚠️ 내용 없음. 클래스만 선언된 빈 파일

---

## 📄 C03ListnerTest.java

```java
package Servlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet(urlPatterns = {"/session/remove","/session/attr/add","/session/attr/replace","/session/attr/remove"})
public class C03ListnerTest extends HttpServlet{

	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("[SERVLET] C03ListnerTest service");
		String uri = req.getRequestURI();
		HttpSession session = req.getSession();
		if(uri.contains("/session/remove")) {
			//세션제거
			session.invalidate();
			
		}else if(uri.contains("/session/attr/add")) {
			session.setAttribute("S_KEY", "S_VAL");
		}else if(uri.contains("/session/attr/replace")) {
			session.setAttribute("S_KEY", "S_VAL_2");
		}else if(uri.contains("/session/attr/remove")) {
			session.removeAttribute("S_KEY");
		}
	}
}
```

---

## 📄 C05ListenerTest.java

```java
package Servlet;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@WebServlet(urlPatterns = {"/request/remove","/request/attr/add","/request/attr/replace","/request/attr/remove"})
public class C05ListenerTest extends HttpServlet{
	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("[SERVLET] C05ListenerTest service");
		String uri = req.getRequestURI();

		if(uri.contains("/request/remove")) {
			//새로운 요청시마다 처리됨
			;
		}else if(uri.contains("/request/attr/add")) {
			req.setAttribute("R_KEY", "R_VAL");
		}else if(uri.contains("/request/attr/replace")) {
			req.setAttribute("R_KEY", "R_VAL_2");
		}else if(uri.contains("/request/attr/remove")) {
			req.removeAttribute("R_KEY");
		}
	}
}
```

---

## 🧠 코드 설명

### ✅ C02ListenerTest

- 애플리케이션(Context) 범위 속성 조작을 통해 `ServletContextAttributeListener` 테스트
    
    - `/app/add` → `setAttribute()`로 새 속성 추가
        
    - `/app/replace` → 같은 key로 새로운 값 설정 → `replace` 감지
        
    - `/app/remove` → `removeAttribute()` 호출
        

---

### ✅ C03ListnerTest

- 세션 객체 조작으로 `HttpSessionListener`, `HttpSessionAttributeListener` 테스트
    
    - `/session/remove` → 세션 무효화 → `sessionDestroyed()`
        
    - `/session/attr/add` → `setAttribute("S_KEY", "S_VAL")`
        
    - `/session/attr/replace` → `setAttribute()` 같은 키 → 값 변경됨
        
    - `/session/attr/remove` → `removeAttribute()`
        

---

### ✅ C05ListenerTest

- 요청(request) 객체의 속성 변경을 통해 `ServletRequestAttributeListener` 테스트
    
    - `/request/attr/add` → `setAttribute("R_KEY", "R_VAL")`
        
    - `/request/attr/replace` → 같은 키로 값 재설정
        
    - `/request/attr/remove` → 해당 키 제거
        
- `request/remove`는 아무 로직 없음 (요청만 발생시키는 용도)
    

---
