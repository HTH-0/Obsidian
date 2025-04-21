
---

## 🛠️ BookDao.java – 도서 DAO 인터페이스

```java
public interface BookDao {
	int insert(BookDto bookDto) throws Exception;
	int update(UserDto userDto) throws SQLException;
	int delete(UserDto userDto) throws SQLException;

		UserDto select(UserDto userDto) throws SQLException;  // 단건 조회
	List<BookDto> selectAll() throws Exception;            // 전체 조회
	List<BookDto> selectAll(int offset, int amount) throws Exception;  // 페이징 처리된 전체 조회
	long count() throws Exception;                         // 전체 건수
	BookDto select(String bookCode) throws Exception;      // 도서 코드로 단건 조회
}
```

### 🧠 인터페이스 설명

- 기본 CRUD 메서드 선언
    
- **`update`, `delete`, `select(UserDto)`는 BookDto가 아니라 UserDto 사용 중** → Book 전용으로 수정 필요
    
- 오버로딩된 `selectAll()` 메서드로 페이징 가능
    
- `count()`는 전체 행 수 반환
    

---

## 🧩 BookDaoImpl.java – 실제 구현 클래스

```java
public class BookDaoImpl implements BookDao {
	private PreparedStatement pstmt;
	private ResultSet rs;
	private ConnectionPool connectionPool;
	private ConnectionItem connectionItem;

	private static BookDao instance;
	private BookDaoImpl() throws ClassNotFoundException, SQLException {
		connectionPool = ConnectionPool.getInstance();
		System.out.println("UserDaoImpl DB Connection Success");
	}
	public static BookDao getInstance() throws ClassNotFoundException, SQLException {
		if(instance==null)
			instance=new BookDaoImpl();
		return instance;
	}
```

### 🧠 주요 메서드 요약

- **insert(BookDto)**  
    → `tbl_book`에 INSERT  
    → 커넥션 풀에서 커넥션 받아 사용 후 반납
    
- **selectAll()**, **selectAll(int offset, int amount)**  
    → 전체 도서 목록 조회 (전체 vs 페이징) → `BookDto` 객체로 변환 후 리스트로 반환
    
- **select(String bookCode)**  
    → 도서 코드 기준으로 단건 조회
    
- **count()**  
    → 도서 전체 수량 반환 (`SELECT COUNT(*)`)
    

---

## 🔧 개선 및 정리 필요 사항

- `update()`와 `delete()`는 `UserDto`를 인자로 받음 → `BookDto`로 바꾸는 게 적절
    
- `BookUpdateController`와 연결하려면 `update(BookDto)`가 반드시 필요함
    
- `delete()` 역시 마찬가지로 `delete(String bookCode)` 형태가 적합
    

---
