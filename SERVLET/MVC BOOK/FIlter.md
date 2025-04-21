
---

# 🛡️ 서블릿 필터 정리

## 1. PermissionFilter.java – 권한 제어 필터

```java
public class PermissionFilter implements Filter {
	private Map<String, Role> pageGradeMap = new HashMap<>();

	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		String projectPath = filterConfig.getServletContext().getContextPath();

		pageGradeMap.put(projectPath + "/", Role.ROLE_ANONYMOUS);
		pageGradeMap.put(projectPath + "/index.do", Role.ROLE_ANONYMOUS);

		// 사용자 관련
		pageGradeMap.put(projectPath + "/user/create", Role.ROLE_ANONYMOUS);
		pageGradeMap.put(projectPath + "/user/login", Role.ROLE_ANONYMOUS);
		pageGradeMap.put(projectPath + "/user/logout", Role.ROLE_ANONYMOUS);

		// 권한별 접근 설정
		pageGradeMap.put(projectPath + "/user/admin", Role.ROLE_ADMIN);
		pageGradeMap.put(projectPath + "/user/manager", Role.ROLE_MANAGER);
		pageGradeMap.put(projectPath + "/user/user", Role.ROLE_USER);

		// 도서 기능 접근 설정
		pageGradeMap.put(projectPath + "/book/list", Role.ROLE_ANONYMOUS);
		pageGradeMap.put(projectPath + "/book/create", Role.ROLE_ANONYMOUS);
		pageGradeMap.put(projectPath + "/book/read", Role.ROLE_ANONYMOUS);
		pageGradeMap.put(projectPath + "/book/update", Role.ROLE_ANONYMOUS);
		pageGradeMap.put(projectPath + "/book/delete", Role.ROLE_ANONYMOUS);
	}

	@Override
	public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain)
			throws IOException, ServletException {

		HttpServletRequest request = (HttpServletRequest) req;
		HttpServletResponse response = (HttpServletResponse) resp;
		HttpSession session = request.getSession();

		String myRole = (String) session.getAttribute("role");

		String uri = request.getRequestURI();
		if (!uri.contains("/resources")) {
			if (myRole == null) {
				session.setAttribute("role", "ROLE_ANONYMOUS");
				response.sendRedirect(request.getContextPath() + "/user/login?msg=not authenticated...");
				return;
			}

			Role my = Role.valueOf(myRole);
			Role pageRole = pageGradeMap.get(uri);

			System.out.printf("URL : %s, MyRole : %d, PageRole : %d\n", uri, my.ordinal(), pageRole.ordinal());

			if (my.ordinal() < pageRole.ordinal()) {
				throw new ServletException("해당 권한으로는 접근이 불가능한 페이지입니다.");
			}

			System.out.println("[FILTER] Perm Filter start..");
		}

		chain.doFilter(req, resp);
		System.out.println("[FILTER] Perm Filter end..");
	}
}
```

### 🧠 설명

- **목적**: 사용자 권한에 따라 접근 가능한 URL을 제한
    
- `pageGradeMap`은 각 URL 별로 필요한 최소 권한(`Role`)을 저장
    
- 로그인하지 않은 사용자에게는 `ROLE_ANONYMOUS` 설정
    
- 현재 로그인한 사용자의 역할보다 페이지에 필요한 역할이 더 높으면 접근 차단
    

---

## 2. UTF8_EncodingFilter.java – 인코딩 필터

```java
public class UTF8_EncodingFilter implements Filter {

	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
			throws IOException, ServletException {

		System.out.println("[Filter] UTF8_EncodingFilter..Start");
		request.setCharacterEncoding("UTF-8");
		chain.doFilter(request, response);
		response.setContentType("text/html; charset=UTF-8");
		System.out.println("[Filter] UTF8_EncodingFilter..End");
	}
}
```

### 🧠 설명

- **목적**: 모든 요청과 응답에 대해 UTF-8 인코딩 설정
    
- 폼 데이터를 제대로 처리하기 위해 `request.setCharacterEncoding("UTF-8")` 설정
    
- `response`도 한글 깨짐 방지를 위해 UTF-8 설정
    

---
