
# 📚 도서 목록 조회 기능

## BookListController.java – 도서 리스트 조회 (with 페이징)

```java
package Controller.book;

import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import Controller.SubController;
import Domain.Dto.BookDto;
import Domain.Dto.Criteria;
import Domain.Dto.PageDto;
import Domain.Service.BookServiceImpl;

public class BookListController implements SubController{
	private HttpServletRequest req;
	private HttpServletResponse resp;
	
	private BookServiceImpl bookService;

	public BookListController() throws Exception{
		this.bookService = BookServiceImpl.getInstance();
	}
	
	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] BookListController execute..");
		
		try {
			//파라미터 
			String pageno = req.getParameter("pageno");
			String amount = req.getParameter("amount");
			String type = req.getParameter("type");
			String keyword = req.getParameter("keyword");
			
			Criteria criteria = null;
			if(pageno == null) {
				criteria = new Criteria(); // 기본값: pageno=1, amount=10
			} else {
				criteria = new Criteria(pageno, 10);
			}
			
			//서비스 호출
			Map<String,Object> serviceResponse = bookService.getAllBooks(criteria);
			Boolean status = (Boolean)serviceResponse.get("status");
			PageDto pageDto = (PageDto)serviceResponse.get("pageDto");
			
			//뷰 처리
			if(status) {
				List<BookDto> list = (List<BookDto>)serviceResponse.get("list");
				req.setAttribute("list", list);
				req.setAttribute("pageDto", pageDto);
			}
			
			req.getRequestDispatcher("/WEB-INF/view/book/list.jsp").forward(req, resp);
	
		} catch(Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			} catch(Exception e2) {}
		}
	}

	private boolean isValid(BookDto bookDto) {
		return true;
	}
	
	// 예외처리 함수
	public void exceptionHandler(Exception e) {
		req.setAttribute("status", false);
		req.setAttribute("message", e.getMessage());
		req.setAttribute("exception", e);
	}
}
```

---

## 🧠 코드 설명

- `BookListController`는 전체 도서 목록을 조회하는 기능 담당 (페이징 포함)
    

### 주요 흐름

- `GET` 요청을 받아 `pageno`, `amount`, `type`, `keyword`를 파라미터로 수집
    
- 파라미터가 없을 경우 기본값(`Criteria` 기본 생성자)으로 설정
    

### 페이징 설정

- `Criteria` 객체를 통해 페이징 정보 설정 (현재 페이지 번호, 페이지 당 아이템 수 등)
    
- 이후 `bookService.getAllBooks(criteria)`로 데이터 조회
    

### 결과 처리

- `status`가 `true`이면 `list`와 `pageDto`를 `request`에 저장하고
    
- `list.jsp`로 포워딩하여 도서 리스트 화면 출력
    

### 예외 처리

- 예외 발생 시 `status`, `message`, `exception` 속성을 담아 `error.jsp`로 포워딩
    

---
