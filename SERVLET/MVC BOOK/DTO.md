
---

# 📦 도서 시스템 DTO 클래스 정리

## 1. BookDto.java – 도서 데이터 객체

```java
public class BookDto {
	private String bookCode;
	private String bookName;
	private String publisher;
	private String isbn;

	public BookDto() {}
	public BookDto(String bookCode, String bookName, String publisher, String isbn) {
		this.bookCode = bookCode;
		this.bookName = bookName;
		this.publisher = publisher;
		this.isbn = isbn;
	}

	// Getter/Setter 및 toString 생략
}
```

### 🧠 설명

- 도서 정보를 담는 기본 DTO 객체
    
- `bookCode`, `bookName`, `publisher`, `isbn` 네 가지 속성을 사용
    
- 생성자, Getter/Setter, `toString()` 포함
    

---

## 2. Criteria.java – 검색 및 페이징 기준 DTO

```java
public class Criteria {
	private int pageno;      // 현재 페이지 번호
	private int amount;      // 페이지 당 표시할 항목 수
	private String type;     // 검색 조건 (예: 도서명, 출판사 등)
	private String keyword;  // 검색 키워드

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
}
```

### 🧠 설명

- 검색 및 페이징 처리를 위한 DTO
    
- `BookListController`에서 파라미터 받아서 `Criteria`로 전달
    
- `type`과 `keyword`는 검색 필터 기능 지원
    

---

## 3. PageDto.java – 페이징 계산 결과 DTO

```java
public class PageDto {
	private long totalCount;       // 총 항목 수
	private int totalpage;         // 총 페이지 수
	private Criteria criteria;     // 현재 페이지 정보

	private int pagePerBlock;      // 한 블럭당 페이지 수
	private int totalBlock;        // 전체 블럭 수
	private int nowBlock;          // 현재 블럭 번호

	private int startPage;         // 블럭 내 시작 페이지
	private int endPage;           // 블럭 내 마지막 페이지

	private boolean prev, next;    // 이전/다음 블럭 존재 여부

	public PageDto(long totalcount, Criteria criteria) {
		this.totalCount = totalcount;
		this.criteria = criteria;

		totalpage = (int)Math.ceil((1.0 * totalcount) / criteria.getAmount());
		pagePerBlock = 15;
		totalBlock = (int)Math.ceil((1.0 * totalpage) / pagePerBlock);
		nowBlock = (int)Math.ceil((1.0 * criteria.getPageno()) / pagePerBlock);

		prev = nowBlock > 1;
		next = nowBlock < totalBlock;

		endPage = (nowBlock * pagePerBlock < totalpage) ? nowBlock * pagePerBlock : totalpage;
		startPage = nowBlock * pagePerBlock - pagePerBlock + 1;
	}
}
```

### 🧠 설명

- `Criteria`를 기준으로 전체 게시물 수를 받아 페이징 계산 처리
    
- `startPage`, `endPage`, `prev`, `next` 등을 통해 페이징 UI 제어에 활용
    
- `BookListController`에서 `PageDto`를 생성하여 JSP에 전달
    

---
