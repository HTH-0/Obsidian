# 📁 09 프로젝트 초기 구조 정리

---

## 📄 SubController.java

```java
package Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface SubController {
	public void execute(HttpServletRequest req, HttpServletResponse resp);
}
```

### 🧠 설명

- **모든 서브 컨트롤러가 구현해야 할 인터페이스**
    
- 프론트 컨트롤러에서 다형성으로 공통 처리하기 위해 사용
    

---

## 📄 HomeController.java

```java
package Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HomeController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		try {
			req.getRequestDispatcher("/WEB-INF/view/index.jsp").forward(req, resp);
		} catch (Exception e) {
			exceptionHandler(e);
		}
	}

	public void exceptionHandler(Exception e) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
}
```

### 🧠 설명

- 루트("/") 또는 "/index.do" 요청 시 호출되는 기본 컨트롤러
    
- `index.jsp` 페이지로 포워딩
    
- 예외 발생 시 에러 정보를 request에 담아 전달
    

---

## 📄 FrontController.java

```java
package Controller;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.ServletConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

//@WebServlet("/")
public class FrontController extends HttpServlet {

	private Map<String, SubController> map = new HashMap<>();

	@Override
	public void init(ServletConfig config) throws ServletException {
		ServletContext context = config.getServletContext();
		try {
			map.put("/", new HomeController());
			map.put("/index.do", new HomeController());
		} catch (Exception e) {
			e.printStackTrace();
			throw new ServletException("서브컨트롤러 등록오류");
		}
	}

	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		try {
			System.out.println("[FC] service...");
			String endPoint = req.getServletPath(); // /index.do 와 같은 endpoint 추출
			System.out.println("[FC] endpoint .." + endPoint);
			SubController controller = map.get(endPoint);
			controller.execute(req, resp);
		} catch (Exception e) {
			e.printStackTrace();
			exceptionHandler(e, req);
			req.getRequestDispatcher("/WEB-INF/view/error.jsp").forward(req, resp);
		}
	}

	public void exceptionHandler(Exception e, HttpServletRequest req) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
}
```

### 🧠 설명

- MVC 패턴의 핵심 구조인 **Front Controller**
    
- 모든 요청을 한 곳에서 받고 URI(endpoint)에 따라 SubController에 위임
    
- 예외 발생 시 공통 에러 페이지로 이동 (`/WEB-INF/view/error.jsp`)
    

---
