---

# 🧩 10번 프로젝트: 사용자/권한별 컨트롤러 정리

---
## # 👤 사용자 회원가입

### 📄 UserCreateController.java

```java
package Controller.user;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;
import Domain.Dto.UserDto;
import Domain.Service.UserServiceImpl;

public class UserCreateController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;
	private UserServiceImpl userService;

	public UserCreateController() throws Exception {
		userService = UserServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] UserCreateController execute..");

		try {
			String uri = req.getMethod();
			if (uri.equals("GET")) {
				req.getRequestDispatcher("/WEB-INF/view/user/create.jsp").forward(req, resp);
				return;
			}

			String username = req.getParameter("username");
			String password = req.getParameter("password");
			String role = "ROLE_USER";

			UserDto userDto = new UserDto(username, password, role);
			boolean isOk = isValid(userDto);
			if (!isOk) {
				req.getRequestDispatcher("/WEB-INF/view/user/create.jsp").forward(req, resp);
				return;
			}

			boolean isJoin = userService.userJoin(userDto);

			if (isJoin) {
				resp.sendRedirect(req.getContextPath() + "/index.do");
			} else {
				req.getRequestDispatcher("/WEB-INF/view/user/join.jsp").forward(req, resp);
			}

		} catch (Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/user/error.jsp").forward(req, resp);
			} catch (Exception e2) {}
		}
	}

	private boolean isValid(UserDto userDto) {
		if (userDto.getUsername() == null || userDto.getUsername().length() <= 4) {
			req.setAttribute("username_err", "userid의 길이는 최소 5자이상이어야합니다");
			return false;
		}
		if (userDto.getUsername().matches("^[0-9].*")) {
			req.setAttribute("username_err", "userid의 첫문자로 숫자가 들어올 수 없습니다");
			return false;
		}
		return true;
	}

	public void exceptionHandler(Exception e) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
}
```

### 🧠 코드 설명

- GET 요청 → 회원가입 폼 렌더링
    
- POST 요청 → 유효성 검사 후 `UserServiceImpl.userJoin()` 호출
    
- 결과에 따라 index 페이지로 이동 또는 join.jsp로 포워딩
    

---

## # 🔐 로그인 / 로그아웃

### 📄 UserLoginController.java

```java
package Controller.user;

import java.io.PrintWriter;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;
import Domain.Dto.UserDto;
import Domain.Service.UserServiceImpl;

public class UserLoginController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;
	private UserServiceImpl userService;

	public UserLoginController() throws Exception {
		userService = UserServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] UserLoginController execute..");

		try {
			String uri = req.getMethod();
			if (uri.equals("GET")) {
				req.getRequestDispatcher("/WEB-INF/view/user/login.jsp").forward(req, resp);
				return;
			}

			String username = req.getParameter("username");
			String password = req.getParameter("password");

			UserDto userDto = new UserDto(username, password, null);
			if (!isValid(userDto)) {
				req.getRequestDispatcher("/WEB-INF/view/user/login.jsp").forward(req, resp);
				return;
			}

			Map<String, Object> serviceResponse = userService.login(userDto, req.getSession());
			boolean isLogin = (boolean) serviceResponse.get("isLogin");
			String message = (String) serviceResponse.get("message");

			if (isLogin) {
				req.getSession().setAttribute("message", message);
				resp.sendRedirect(req.getContextPath() + "/index.do");
			} else {
				req.getRequestDispatcher("/WEB-INF/view/user/login.jsp").forward(req, resp);
			}

		} catch (Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/user/error.jsp").forward(req, resp);
			} catch (Exception e2) {}
		}
	}

	private boolean isValid(UserDto userDto) {
		if (userDto.getUsername() == null || userDto.getUsername().length() <= 4) return false;
		if (userDto.getUsername().matches("^[0-9].*")) return false;
		return true;
	}

	public void exceptionHandler(Exception e) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
}
```

### 📄 UserLogoutController.java

```java
package Controller.user;

import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import Controller.SubController;
import Domain.Service.UserServiceImpl;

public class UserLogoutController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;
	private UserServiceImpl userService;

	public UserLogoutController() throws Exception {
		userService = UserServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] UserLogoutController execute..");

		try {
			HttpSession session = req.getSession();
			Boolean isAuth = (Boolean) session.getAttribute("isAuth");
			String role = (String) session.getAttribute("role");
			String username = (String) session.getAttribute("username");

			if (isAuth == null || !isAuth) {
				req.setAttribute("message", "로그인된 상태가 아닙니다");
				req.getRequestDispatcher("/WEB-INF/view/user/login.jsp").forward(req, resp);
				return;
			}

			Map<String, Object> serviceResponse = userService.logout(req.getSession());
			Boolean isLogout = (Boolean) serviceResponse.get("isLogout");

			HttpSession reSession = req.getSession(true);
			reSession.setAttribute("message", isLogout ? "로그아웃 성공!" : "로그아웃 실패!");

			resp.sendRedirect(req.getContextPath() + "/user/login");

		} catch (Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/user/error.jsp").forward(req, resp);
			} catch (Exception e2) {}
		}
	}

	public void exceptionHandler(Exception e) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
}
```

### 🧠 코드 설명

- 로그인 시 세션에 사용자 인증 정보 저장
    
- 로그아웃 시 세션 무효화 및 리다이렉션
    
- 로그인 상태가 아니면 로그인 페이지로 이동
    

---

## # 🧑‍💼 권한별 메인 컨트롤러

### 📄 UserMainController.java

```java
package Controller.user;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;

public class UserMainController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		try {
			req.getRequestDispatcher("/WEB-INF/view/user/user.jsp").forward(req, resp);
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

### 📄 ManagerMainController.java

```java
package Controller.user;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;

public class ManagerMainController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		try {
			req.getRequestDispatcher("/WEB-INF/view/user/manager.jsp").forward(req, resp);
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

### 📄 AdminMainController.java

```java
package Controller.user;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;

public class AdminMainController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		try {
			req.getRequestDispatcher("/WEB-INF/view/user/admin.jsp").forward(req, resp);
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

---

## ✅ 전체 흐름 요약

- **회원가입**: `/user/create`
    
- **로그인**: `/user/login` → 세션 저장
    
- **로그아웃**: `/user/logout` → 세션 제거
    
- **권한 메인 페이지**: `/user/main`, `/manager/main`, `/admin/main` 으로 연결
    

---

