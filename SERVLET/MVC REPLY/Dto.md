
---

## 🧾 BookReplyDto.java

```java
package Domain.Dto;

import java.time.LocalDateTime;

public class BookReplyDto {
	private int no;
	private String bookCode;
	private String username;
	private String contents;
	private LocalDateTime createAt;

	public BookReplyDto() {}

	public BookReplyDto(int no, String bookCode, String username, String contents, LocalDateTime createAt) {
		this.no = no;
		this.bookCode = bookCode;
		this.username = username;
		this.contents = contents;
		this.createAt = createAt;
	}

	public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public String getBookCode() {
		return bookCode;
	}
	public void setBookCode(String bookCode) {
		this.bookCode = bookCode;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getContents() {
		return contents;
	}
	public void setContents(String contents) {
		this.contents = contents;
	}
	public LocalDateTime getCreateAt() {
		return createAt;
	}
	public void setCreateAt(LocalDateTime createAt) {
		this.createAt = createAt;
	}

	@Override
	public String toString() {
		return "BookReplyDto [no=" + no + ", bookCode=" + bookCode + ", username=" + username + ", contents=" + contents
				+ ", createAt=" + createAt + "]";
	}
}
```

### 🧠 코드 설명

- 댓글 데이터를 저장하는 DTO
    
- 필드 구성: 댓글 번호(`no`), 도서 코드(`bookCode`), 작성자(`username`), 내용(`contents`), 작성 시각(`createAt`)
    
- `toString()` 재정의: 디버깅 및 로그에 유용
    

---

## 📄 Criteria.java

```java
package Domain.Dto;

public class Criteria {
	private int pageno; 		// 현재 페이지 번호
	private int amount;		// 페이지당 항목 수
	private String type;		// 검색 타입 (예: 도서명, 도서코드 등)
	private String keyword;	// 검색 키워드

	public Criteria() {
		this.pageno = 1;
		this.amount = 10;
	}
	
	public Criteria(String pageno, int amount, String type, String keyword) {
		this.pageno = Integer.parseInt(pageno);
		this.amount = amount;
		this.type = type;
		this.keyword = keyword;
	}
	
	public Criteria(String pageno, int amount) {
		this.pageno = Integer.parseInt(pageno);
		this.amount = amount;
	}

	public int getPageno() {
		return pageno;
	}
	public void setPageno(int pageno) {
		this.pageno = pageno;
	}
	public int getAmount() {
		return amount;
	}
	public void setAmount(int amount) {
		this.amount = amount;
	}
	public String getType() {
		return type;
	}
	public void setType(String type) {
		this.type = type;
	}
	public String getKeyword() {
		return keyword;
	}
	public void setKeyword(String keyword) {
		this.keyword = keyword;
	}

	@Override
	public String toString() {
		return "Criteria [pageno=" + pageno + ", amount=" + amount + ", type=" + type + ", keyword=" + keyword + "]";
	}
}
```

### 🧠 코드 설명

- 페이징 및 검색 조건을 관리하는 DTO
    
- `BookListController`에서 사용
    
- 기본 생성자: 페이지 1, 10개 항목
    
- 오버로딩된 생성자에서 `pageno`는 `String`으로 받아서 `int`로 변환함
    

---

## 🧩 전체 구조 요약

### 📌 댓글 등록

1. 클라이언트 요청 → `BookReplyCreateController`
    
2. 세션에서 로그인 정보 확인
    
3. `BookReplyDto` 생성 → `bookService.bookReplyAdd(dto)`
    
4. DAO (`BookReplyDaoImpl.insert`) → DB에 저장
    
5. JSON 응답 반환
    

---

### 📌 댓글 조회

1. 클라이언트 요청 → `BookReplyListController`
    
2. `bookCode` 파라미터 수신
    
3. DAO에서 `selectAll()` + `count()` 호출
    
4. 댓글 목록 + 개수를 JSON으로 응답
    

---

### 📌 도서 목록 조회 (페이징 + 검색)

1. 클라이언트 요청 → `BookListController`
    
2. `Criteria` 객체로 페이지번호, 검색타입/키워드 구성
    
3. `bookService.getAllBooks(criteria)` 호출
    
4. 결과를 JSP로 포워딩 (`list.jsp`)
    

---

