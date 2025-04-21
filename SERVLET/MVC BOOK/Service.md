
---

# 🧠 BookServiceImpl.java – 도서 서비스 클래스

```java
public class BookServiceImpl {
	private BookDao bookDao;

	// 싱글톤 패턴
	private static BookServiceImpl instance;
	private BookServiceImpl() throws Exception {	
		bookDao = BookDaoImpl.getInstance();
	}
	public static BookServiceImpl getInstance() throws Exception {
		if (instance == null)
			instance = new BookServiceImpl();
		return instance;
	}
```

### 📌 싱글톤 패턴

- `BookDaoImpl`과 연결됨
    
- 생성자에서 DAO 인스턴스를 주입받음
    

---

## ✅ 도서 등록

```java
public boolean bookRegistration(BookDto bookDto) throws Exception {
	int result = bookDao.insert(bookDto);
	return result > 0;
}
```

- 단순 삽입 로직. 등록 성공 여부 `boolean`으로 반환
    

---

## 📄 도서 전체 조회 (단순)

```java
public Map<String,Object> getAllBooks() throws Exception {
	Map<String,Object> response = new LinkedHashMap<>();
	List<BookDto> list = bookDao.selectAll();
	if (list.size() > 0) {
		response.put("status", true);
		response.put("list", list);
	} else {
		response.put("status", false);
	}
	return response;
}
```

- 전체 도서 리스트만 조회 (페이징 없음)
    
- `status` 키로 결과 성공 여부 판단
    

---

## 📄 도서 전체 조회 (Criteria 기반 페이징 포함)

```java
public Map<String, Object> getAllBooks(Criteria criteria) throws Exception {
	Map<String,Object> response = new LinkedHashMap<>();
	int offset = (criteria.getPageno() - 1) * criteria.getAmount();

	List<BookDto> list = bookDao.selectAll(offset, criteria.getAmount());

	long totalCount = bookDao.count();
	PageDto pageDto = new PageDto(totalCount, criteria);

	if (list.size() > 0) {
		response.put("status", true);
		response.put("list", list);
		response.put("pageDto", pageDto);
	} else {
		response.put("status", false);
	}
	return response;
}
```

- `offset` 계산 후 페이징된 도서 리스트 조회
    
- 전체 건수로 `PageDto` 구성 → JSP에서 페이징 처리 가능
    

---

## 🔍 도서 상세 조회

```java
public Map<String, Object> getBook(String bookCode) throws Exception {
	Map<String, Object> response = new LinkedHashMap<>();
	BookDto bookDto = bookDao.select(bookCode);

	if (bookDto == null)
		response.put("status", false);
	else {
		response.put("status", true);
		response.put("bookDto", bookDto);
	}
	return response;
}
```

- `bookCode`로 단건 조회
    
- 결과가 존재하면 `bookDto` 포함한 응답 반환
    

---

## ⚠️ 도서 수정 (미구현)

```java
public Map<String, Object> modifyBook(BookDto bookDto) {
	// TODO Auto-generated method stub
	return null;
}
```

- `BookUpdateController`에서 호출하는 핵심 메서드인데 아직 구현되지 않음
    
- `bookDao`에 `update(BookDto)` 구현 후 이 메서드에서 호출 필요
    

---

## ✅ 다음 정리 또는 작업 추천

- `BookDao`에 `update(BookDto)`, `delete(String bookCode)` 메서드 추가
    
- `BookServiceImpl.modifyBook()` 실제 구현
    
- `BookDeleteController` 구현 시 `removeBook(String bookCode)`도 서비스 계층에 추가 필요
    

