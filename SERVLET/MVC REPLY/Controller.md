---

# 📘 도서 관련 컨트롤러 정리

---

## BookReplyCreateController.java

```java
package Controller.book;

import java.io.PrintWriter;
import java.time.LocalDateTime;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import Controller.SubController;
import Domain.Dto.BookReplyDto;
import Domain.Service.BookServiceImpl;

public class BookReplyCreateController implements SubController{
	private HttpServletRequest req;
	private HttpServletResponse resp;	
	
	private BookServiceImpl bookService;

	public BookReplyCreateController() throws Exception{
		this.bookService = BookServiceImpl.getInstance();
	}

	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] BookReplyCreateController execute..");
		
		try {
			String bookCode = req.getParameter("bookCode");
			String username = null;
			String contents = req.getParameter("contents");
			LocalDateTime createAt = LocalDateTime.now();
			
			HttpSession session = req.getSession();
			username = (String)session.getAttribute("username");
			if(username == null)
				throw new ServletException("로그인이 필요합니다!");
			
			BookReplyDto dto = new BookReplyDto(-1, bookCode, username, contents, createAt);

			if(!isValid(dto))
				;

			boolean isAdded = bookService.bookReplyAdd(dto);

			PrintWriter out = resp.getWriter();
			if(isAdded) {
				out.println("{\"message\":\"success!!!\"}");
			} else {
				out.println("{\"message\":\"fail...\"}");
			}
				
		} catch(Exception e) {
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			} catch(Exception e2) {}
		}
	}

	private boolean isValid(BookReplyDto dto) {
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

- 사용자가 작성한 댓글을 서버에 저장하는 컨트롤러
    
- 세션에서 로그인 여부 확인 (`username`)
    
- 댓글 내용을 `BookReplyDto`로 포장 후 서비스 호출
    
- 성공 여부를 JSON 형식으로 응답
    

---

## BookReplyListController.java

```java
package Controller.book;

import java.io.PrintWriter;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;

import Controller.SubController;
import Domain.Dto.BookReplyDto;
import Domain.Service.BookServiceImpl;

public class BookReplyListController implements SubController{
	private HttpServletRequest req;
	private HttpServletResponse resp;
	
	private BookServiceImpl bookService;

	public BookReplyListController() throws Exception{
		this.bookService = BookServiceImpl.getInstance();
	}
	
	@Override
	public void execute(HttpServletRequest req, HttpServletResponse resp) {
		this.req = req;
		this.resp = resp;
		System.out.println("[SC] BookServiceImpl execute..");
	
		try {
			String bookCode = req.getParameter("bookCode");

			if(!isValid(bookCode))
				;

			List<BookReplyDto> serviceResponse = bookService.getAllBookReply(bookCode);
			long cnt = bookService.bookReplyCount(bookCode);
			
			Map<String,Object> responseEntity = new LinkedHashMap<>();
			responseEntity.put("replyCnt", cnt);
			responseEntity.put("replyList", serviceResponse);

			resp.setContentType("application/json");
			ObjectMapper objectMapper = new ObjectMapper();
			objectMapper.registerModule(new JavaTimeModule());
			objectMapper.disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS);

			String JsonData = objectMapper.writeValueAsString(responseEntity);
			PrintWriter out = resp.getWriter();
			out.write(JsonData);
	
		}catch(Exception e) {
			e.printStackTrace();
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			}catch(Exception e2) {}
		}
	}

	private boolean isValid(String bookCode) {
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

- 특정 도서에 달린 댓글 리스트를 가져와 JSON 형태로 응답
    
- `bookCode`를 기준으로 댓글 개수(`replyCnt`)와 리스트(`replyList`) 반환
    
- `ObjectMapper`로 JSON 직렬화 처리 (`LocalDateTime` 처리 위해 `JavaTimeModule` 사용)
    

---

## BookListController.java

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
			String pageno = req.getParameter("pageno");
			String amount = req.getParameter("amount");
			String type = req.getParameter("type");
			String keyword = req.getParameter("keyword");
			
			Criteria criteria = (pageno == null) 
				? new Criteria() 
				: new Criteria(pageno, 10, type, keyword);

			Map<String,Object> serviceResponse = bookService.getAllBooks(criteria);
			Boolean status = (Boolean)serviceResponse.get("status");
			PageDto pageDto = (PageDto)serviceResponse.get("pageDto");

			if(status) {
				List<BookDto> list = (List<BookDto>)serviceResponse.get("list");
				req.setAttribute("list", list);
				req.setAttribute("pageDto", pageDto);
			}

			req.getRequestDispatcher("/WEB-INF/view/book/list.jsp").forward(req, resp);
	
		}catch(Exception e) {
			e.printStackTrace();
			exceptionHandler(e);
			try {
				req.getRequestDispatcher("/WEB-INF/view/book/error.jsp").forward(req, resp);
			}catch(Exception e2) {}
		}
	}

	private boolean isValid(BookDto bookDto) {
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

- 도서 목록 조회 컨트롤러
    
- `Criteria` 객체를 통해 페이지 번호, 검색 조건 등 설정
    
- 결과는 `list.jsp` 뷰로 전달하며, 페이징 정보는 `PageDto`를 통해 전달
    
- 뷰 렌더링은 서버 측 JSP에서 처리
    

---
