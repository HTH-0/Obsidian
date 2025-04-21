# 📖 도서 상세 조회 기능

## BookReadController.java – 특정 도서 상세 보기

```java
package Controller.book;

import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
	
import Controller.SubController;
import Domain.Service.BookServiceImpl;

public class BookReadController implements SubController{
	private HttpServletRequest req;
	private HttpServletResponse resp;
	
	private BookServiceImpl bookService;

	public BookReadController() throws Exception{
		this.bookService = BookServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] BookReadController execute..");
		
		try {
			//파라미터
			String bookCode = req.getParameter("bookCode");
			
			//유효성
			if(!isValid(bookCode)) {
				resp.sendRedirect(req.getContextPath() + "/book/list");
			}

			//서비스
			Map<String,Object> serviceResponse = bookService.getBook(bookCode);
			Boolean status = (Boolean)serviceResponse.get("status");

			if(status)
				req.setAttribute("bookDto", serviceResponse.get("bookDto"));
			
			//뷰
			req.getRequestDispatcher("/WEB-INF/view/book/read.jsp").forward(req, resp);
			
		}catch(Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			}catch(Exception e2) {}
		}
	}

	private boolean isValid(String bookCode) {
		if(bookCode.isEmpty()) {
			req.setAttribute("bookCode", "BookCode 유효성 오류");
		}
		return true;		
	}
	
	// 예외처리함수
	public void exceptionHandler(Exception e) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
}
```

---

## 🧠 코드 설명

- `BookReadController`는 특정 도서의 상세 정보를 조회하는 기능 담당
    

### 요청 파라미터 처리

- `bookCode`를 `request`로부터 받아옴
    
- 도서 개별 조회에 필요한 고유 식별자
    

### 유효성 검사

- `bookCode`가 비어있는 경우 오류 메시지 설정
    
- 현재는 `true`를 리턴하고 있음 → 실제 검증 로직 강화 필요
    

### 서비스 호출

- `bookService.getBook(bookCode)` 호출
    
- 반환된 `Map`에서 `status` 확인
    
- `status=true`면 도서 정보(`bookDto`)를 `request`에 저장
    

### 뷰 처리

- `read.jsp`로 포워딩하여 도서 상세 정보 출력
    
- 실패 시 → 예외 처리 후 `error.jsp`로 이동
    

---

다음은 `BookUpdateController.java` 정리 들어갈게.

# ✏️ 도서 정보 수정 기능

## BookUpdateController.java – 도서 정보 수정

```java
package Controller.book;

import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;
import Domain.Dto.BookDto;
import Domain.Service.BookServiceImpl;

public class BookUpdateController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;
	private BookServiceImpl bookService;

	public BookUpdateController() throws Exception {
		this.bookService = BookServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] BookUpdateController execute..");

		try {
			// 파라미터
			String bookCode = req.getParameter("bookCode");

			BookDto bookDto = null;

			// 유효성
			if (!isValid(bookDto)) {
				req.getRequestDispatcher("/WEB-INF/view/book/read.jsp?bookCode=" + bookCode).forward(req, resp);
			}

			// 서비스
			Map<String, Object> serviceResponse = bookService.modifyBook(bookDto);

			// 뷰
			resp.sendRedirect(req.getContextPath() + "/book/list");

		} catch (Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			} catch (Exception e2) {
			}
		}

	}

	private boolean isValid(BookDto bookDto) {
		if (true) {
			;
		}
		return true;
	}

	// 예외처리함수
	public void exceptionHandler(Exception e) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
	
}
```

---

## 🧠 코드 설명

- `BookUpdateController`는 도서 수정 기능 담당
    

### 구조 및 흐름

- `execute()` 내부에서 `bookCode`를 파라미터로 받고 있음
    
- 수정 대상 도서의 정보를 담는 `BookDto bookDto`는 **현재 null 상태** → 반드시 보완 필요
    

### 유효성 검사 (`isValid`)

- 현재 로직은 의미 없는 코드로 구성됨 → 실제 값 검증 로직 필요
    

### 서비스 처리

- `bookService.modifyBook(bookDto)` 호출
    
- 결과와 무관하게 목록 페이지(`/book/list`)로 리다이렉트 처리
    

### 예외 처리

- 에러 발생 시 `request`에 상태, 메시지, 예외를 저장하고 `error.jsp`로 포워딩 시도
    

---

⚠️ **수정 필요 사항 요약**

- `bookDto` 값 수집 코드 누락 → `req.getParameter()` 통해 개별 값 수집 필요
    
- 유효성 검사 로직 구현 필요 (`null` 방지 및 필수 항목 검사)
    

---

이제 마지막으로 `BookDeleteController.java` 확인할게.

# 🗑️ 도서 삭제 기능

## BookDeleteController.java – 도서 삭제 

```java
package Controller.book;

public class BookDeleteController {

}
```

---

## 🧠 코드 설명

- 이 컨트롤러는 도서 삭제 기능을 위한 클래스지만, **아직 구현되지 않음**
    
- `SubController` 인터페이스를 구현하지 않았고, `execute()` 메서드도 없음
    

### 향후 구현 방향 제안

- 다음과 같은 형태로 작성될 수 있음:
    

```java
public class BookDeleteController implements SubController {
	private HttpServletRequest req;
	private HttpServletResponse resp;
	private BookServiceImpl bookService;

	public BookDeleteController() throws Exception {
		this.bookService = BookServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;

		try {
			String bookCode = req.getParameter("bookCode");

			if (bookCode == null || bookCode.isEmpty()) {
				resp.sendRedirect(req.getContextPath() + "/book/list");
				return;
			}

			boolean result = bookService.removeBook(bookCode);

			if (result) {
				resp.sendRedirect(req.getContextPath() + "/book/list");
			} else {
				req.setAttribute("message", "삭제 실패");
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			}
		} catch (Exception e) {
			req.setAttribute("status", false);
			req.setAttribute("message", e.getMessage());
			req.setAttribute("exception", e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			} catch (Exception e2) {
			}
		}
	}
}
```

---
