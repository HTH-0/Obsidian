# 🚪 사용자 진입 흐름 관련 서블릿/컨트롤러 정리

---

## 📄 FrontController.java

```java
package Controller;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class FrontController extends HttpServlet {
	private Map<String, SubController> map = new HashMap<>();

	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("[FC] service ..");
		String endPoint = req.getRequestURI();
		System.out.println("[FC] endpoint .." + endPoint);
	}

	@Override
	public void init(ServletConfig config) throws ServletException {
		map.put("/user/create", new UserCreateController());
		// 추후 도서 컨트롤러도 여기에 등록 예정
	}
}
```

---

## 📄 Home.java

```java
package Servlets;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(urlPatterns = {"/index.do", "/main.do"})
public class Home extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		String uri = req.getRequestURI();
		if (uri.contains("/index.do")) {
			System.out.println("GET /index.do");
			req.getRequestDispatcher("/WEB-INF/index.jsp").forward(req, resp);
		} else {
			System.out.println("GET /main.do");
			req.getRequestDispatcher("/WEB-INF/main.jsp").forward(req, resp);
		}
	}
}
```

---

## 📄 Join.java

```java
package Servlets;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Utils.MysqlDbUtils;
import Utils.UserDto;

@WebServlet("/join.do")
public class Join extends HttpServlet {

	private MysqlDbUtils dbutils;

	@Override
	public void init() throws ServletException {
		try {
			dbutils = MysqlDbUtils.getInstance();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("GET /join.do");
		req.getRequestDispatcher("/WEB-INF/user/join.jsp").forward(req, resp);
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("POST /join.do");

		String username = req.getParameter("username");
		String password = req.getParameter("password");

		int result = 0;
		try {
			result = dbutils.insert(new UserDto(username, password, "ROLE_USER"));
		} catch (Exception e) {
			e.printStackTrace();
		}

		if (result > 0) {
			resp.sendRedirect(req.getContextPath() + "/login.do");
		} else {
			req.getRequestDispatcher("/WEB-INF/user/join.jsp").forward(req, resp);
		}
	}
}
```

---

## 📄 Login.java

```java
package Servlets;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import Utils.MysqlDbUtils;
import Utils.UserDto;

@WebServlet("/login.do")
public class Login extends HttpServlet {

	private MysqlDbUtils dbutils;

	@Override
	public void init() throws ServletException {
		try {
			dbutils = MysqlDbUtils.getInstance();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("GET /login.do");
		req.getRequestDispatcher("/WEB-INF/user/login.jsp").forward(req, resp);
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("POST /login.do");

		String username = req.getParameter("username");
		String password = req.getParameter("password");

		boolean isAuth = false;
		try {
			UserDto dbUser = dbutils.selectOne(username);
			if (dbUser != null && dbUser.getPassword().equals(password)) {
				HttpSession session = req.getSession();
				session.setAttribute("username", username);
				session.setAttribute("role", dbUser.getRole());
				isAuth = true;
			}
		} catch (Exception e) {
			e.printStackTrace();
		}

		if (isAuth) {
			resp.sendRedirect(req.getContextPath() + "/main.do");
		} else {
			req.getRequestDispatcher("/WEB-INF/user/login.jsp").forward(req, resp);
		}
	}
}
```

---

## 🧠 코드 설명

### ✅ FrontController

- MVC 패턴에서 **요청 진입 지점**
    
- 모든 요청 URI를 기준으로 `Map<String, SubController>`에서 적절한 서브컨트롤러 실행하도록 구성
    
- 현재는 `/user/create`만 등록, 확장 구조
    

---

### ✅ Home

- `/index.do` → `/WEB-INF/index.jsp`
    
- `/main.do` → `/WEB-INF/main.jsp`
    
- 단순 뷰 연결을 위한 라우터 역할
    

---

### ✅ Join

- `/join.do`로 회원가입 요청 처리
    
- GET: 가입 폼 JSP 연결
    
- POST: DB에 유저 등록 후 성공 시 `/login.do`로 리다이렉트
    
- `MysqlDbUtils`로 DB작업 수행
    

---

### ✅ Login

- `/login.do` 요청 처리
    
- GET: 로그인 폼 JSP 연결
    
- POST: DB에서 사용자 조회 및 세션 저장
    
- 로그인 성공 시 `/main.do`, 실패 시 로그인 페이지로 포워딩
    

---
