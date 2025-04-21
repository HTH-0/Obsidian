# 📚 도서 관리 기능 컨트롤러 정리

## BookCreateController.java – 도서 등록 기능

```java
package Controller.book;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;
import Domain.Dto.BookDto;
import Domain.Service.BookServiceImpl;

public class BookCreateController implements SubController{
	private HttpServletRequest req;
	private HttpServletResponse resp;
	
	
	private BookServiceImpl bookService;

	public BookCreateController() throws Exception{
		this.bookService = BookServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] BookCreateController execute..");
		
		try {
			String uri = req.getMethod();
			
			if(uri.equals("GET")) {
				req.getRequestDispatcher("/WEB-INF/view/book/create.jsp").forward(req, resp);
				return ;
			}
	
			//파라미터
			String bookCode = req.getParameter("bookCode");
			String bookName = req.getParameter("bookName");
			String publisher = req.getParameter("publisher");
			String isbn = req.getParameter("isbn");
			
			BookDto bookDto = new BookDto(bookCode,bookName,publisher,isbn);
			//유효성
			if(!isValid(bookDto)) {
				req.getRequestDispatcher("/WEB-INF/view/book/create.jsp").forward(req, resp);
				return ;
			}
			//서비스
			boolean isadded =  bookService.bookRegistration(bookDto);
			
			//뷰
			if(isadded) {
				resp.sendRedirect(req.getContextPath()+"/book/list");
			}else {
				req.getRequestDispatcher("/WEB-INF/view/book/create.jsp").forward(req, resp);
			}
			
		}catch(Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			}catch(Exception e2) {}
		}

	}

	private boolean isValid(BookDto bookDto) {
		if(bookDto.getBookCode().isEmpty()) {
			req.setAttribute("bookCode", "BookCode를 입력하세요");
		}
		if(bookDto.getBookName().isEmpty()) {
			req.setAttribute("bookName", "BookName를 입력하세요");
		}
		if(bookDto.getPublisher().isEmpty()) {
			req.setAttribute("publisher", "출판사명을 입력하세요");
		}
		if(bookDto.getIsbn().isEmpty()) {
			req.setAttribute("isbn", "isbn을 입력하세요");
		}		
		if(
				bookDto.getBookCode().isEmpty() || 
				bookDto.getBookName().isEmpty() ||
				bookDto.getPublisher().isEmpty() ||
				bookDto.getIsbn().isEmpty()
				)
			return false;
		
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

- `BookCreateController`는 도서 등록(Create) 기능을 담당하는 컨트롤러 클래스
    

### 클래스 선언 및 생성자

- `SubController` 인터페이스를 구현
    
- `bookService`는 싱글톤 방식으로 생성된 `BookServiceImpl` 인스턴스를 사용
    

### `execute` 메서드

- `req`, `resp`를 필드에 저장하고 `HTTP Method` 확인
    
- **GET 요청**이면 도서 등록 폼 페이지로 포워딩 (`create.jsp`)
    
- **POST 요청**이면 아래 작업 수행
    

### 파라미터 처리

- `req.getParameter()`로 사용자 입력값(도서 코드, 이름, 출판사, ISBN) 수집
    
- `BookDto` 객체로 매핑
    

### 유효성 검사 (`isValid`)

- 각 필드가 비어있는지 검사하고, 비어있으면 해당 필드명에 에러 메시지를 `request attribute`로 저장
    
- 하나라도 비어있으면 등록 페이지로 포워딩
    

### 서비스 로직 호출

- `bookService.bookRegistration(bookDto)` 호출하여 DB에 도서 정보 저장 시도
    
- 결과에 따라:
    
    - 성공 시 → 도서 목록 페이지(`/book/list`)로 리다이렉트
        
    - 실패 시 → 다시 등록 페이지로 포워딩
        

### 예외 처리 (`exceptionHandler`)

- 예외 발생 시 상태, 메시지, 예외 객체를 `request attribute`로 저장
    
- `error.jsp`로 포워딩 시도
    

---
