
---

## 🗃️ BookReplyDaoImpl.java

```java
package Domain.Dao;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.LinkedList;
import java.util.List;

import Domain.Dto.BookDto;
import Domain.Dto.BookReplyDto;

public class BookReplyDaoImpl extends Dao {

	// 싱글톤 패턴
	private static BookReplyDaoImpl instance;
	private BookReplyDaoImpl() throws Exception {
		System.out.println("[DAO] BookReplyDaoImpl init...");		
	};
	public static BookReplyDaoImpl getInstance() throws Exception {
		if(instance==null)
			instance = new BookReplyDaoImpl();
		return instance;
	}
	
	// 댓글 등록
	public int insert(BookReplyDto dto) throws Exception {
		try {
			connectionItem = connectionPool.getConnection();
			Connection conn = connectionItem.getConn();
			
			pstmt = conn.prepareStatement("insert into tbl_reply values(null,?,?,?,?)");
			pstmt.setString(1, dto.getBookCode());
			pstmt.setString(2, dto.getUsername());
			pstmt.setString(3, dto.getContents());
			pstmt.setTimestamp(4, Timestamp.valueOf(dto.getCreateAt()));
			
			return pstmt.executeUpdate();
			
		}catch(SQLException e) {
			e.printStackTrace();
			throw new SQLException("BOOKDAO's INSERT SQL EXCEPTION!!");
		}finally {
			try {pstmt.close();}catch(Exception e2) {}
			connectionPool.releaseConnection(connectionItem);
		}
	}

	// 댓글 목록 조회
	public List<BookReplyDto> selectAll(String bookCode)  throws Exception {
		List<BookReplyDto> list = new LinkedList<>();
		BookReplyDto dto = null;
		try {
			connectionItem = connectionPool.getConnection();
			Connection conn = connectionItem.getConn();		
			pstmt = conn.prepareStatement("select * from tbl_reply where bookCode=? order by no desc");
			pstmt.setString(1, bookCode);
			
			rs = pstmt.executeQuery();
			if(rs != null) {
				while(rs.next()) {
					dto = new BookReplyDto();
					dto.setNo(rs.getInt(1));
					dto.setBookCode(rs.getString(2));
					dto.setUsername(rs.getString(3));
					dto.setContents(rs.getString(4));
					dto.setCreateAt(rs.getTimestamp(5).toLocalDateTime());
					
					list.add(dto);
				}
			}
			return list;
		}catch(SQLException e) {
			e.printStackTrace();
			throw new SQLException("BOOKDAO's SELECT SQL EXCEPTION!!");
		}finally {
			try {pstmt.close();}catch(Exception e2) {}
			connectionPool.releaseConnection(connectionItem);	
		}
	}
	
	// 댓글 개수 조회
	public long count(String bookCode) throws Exception {
		long count = 0;
		try {
			connectionItem = connectionPool.getConnection();
			Connection conn = connectionItem.getConn();
			
			pstmt = conn.prepareStatement("select count(*) from tbl_reply where bookCode=?");
			pstmt.setString(1, bookCode);
			
			rs = pstmt.executeQuery();
			
			if(rs != null && rs.next())
				count = rs.getLong(1);
				
			return count;
			
		}catch(SQLException e) {
			e.printStackTrace();
			throw new SQLException("BOOKDAO's SELECT SQL EXCEPTION!!");
		}finally {
			try {pstmt.close();}catch(Exception e2) {}
			connectionPool.releaseConnection(connectionItem);	
		}
	}
}
```

---

### 🧠 코드 설명

- `BookReplyDaoImpl extends Dao`: 커스텀 DAO 기반 구현. `connectionPool`, `pstmt`, `rs` 사용
    
- `getInstance()`: 싱글톤 패턴. DAO 객체는 시스템 전역에서 하나만 생성
    
- `insert(BookReplyDto)`: 댓글을 `tbl_reply` 테이블에 저장
    
    - `createAt`은 `Timestamp.valueOf(LocalDateTime)`으로 변환
        
- `selectAll(String bookCode)`: 특정 도서의 댓글 리스트 조회
    
    - 최신 순으로 정렬
        
- `count(String bookCode)`: 해당 도서 댓글 개수 반환
    

---

이제 댓글 등록/조회 기능의 전체 흐름은 다음과 같아:

```
📌 댓글 등록
사용자 요청 → BookReplyCreateController → BookServiceImpl.bookReplyAdd()
→ BookReplyDaoImpl.insert() → DB 저장 → JSON 응답

📌 댓글 조회
사용자 요청 → BookReplyListController → BookServiceImpl.getAllBookReply(), bookReplyCount()
→ BookReplyDaoImpl.selectAll(), count() → JSON 응답
```
