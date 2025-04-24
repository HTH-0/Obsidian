# 📝 메모 DAO 구현 클래스 (DB 저장 기능)

## 📦 전체 코드

```java
package com.example.app.domain.Dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.sql.Timestamp;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import com.example.app.domain.Dto.MemoDto;

@Repository
public class MemoDaoImpl {
	
	@Autowired
	private DataSource dataSource;
	
	public int insert(MemoDto memoDto) throws SQLException {
		Connection con = dataSource.getConnection();
		PreparedStatement pstmt = con.prepareStatement("Insert into tbl_memo values(?,?,?,?)");
		pstmt.setInt(1, memoDto.getId());
		pstmt.setString(2, memoDto.getText());
		pstmt.setString(3, memoDto.getWriter());
		pstmt.setTimestamp(4, Timestamp.valueOf(memoDto.getCreateAt()));
		int result = pstmt.executeUpdate();
		
		return result;
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Repository`:
    
    - DAO 계층 클래스임을 나타냄.
        
    - 스프링이 해당 클래스를 빈으로 인식하고 예외 변환(AOP 기반)도 지원함.
        

### ✅ 주요 메서드 설명

- `insert(MemoDto memoDto)`:
    
    - 메모 데이터를 DB에 저장하는 역할.
        
    - SQL: `"Insert into tbl_memo values(?,?,?,?)"`
        
        - 순서대로 `id`, `text`, `writer`, `createAt` 필드 입력.
            
    - `executeUpdate()` 결과값은 영향받은 행 수 (1 이상이면 성공).
        

### ✅ 의존성 주입 (DI)

- `@Autowired private DataSource dataSource`:
    
    - 커넥션 풀에서 DB 연결을 위한 DataSource 객체 자동 주입.
        

### ✅ 기타 주요 로직 설명

- `Timestamp.valueOf(memoDto.getCreateAt())`:
    
    - `LocalDateTime` → `Timestamp` 변환.
        
    - DB `DATETIME` 타입 컬럼에 적합한 포맷으로 변환 처리.
        
- 연결 해제(close)는 없음:
    
    - 현재 코드엔 `Connection`, `PreparedStatement`를 닫는 `close()`가 빠져있음.
        
    - 누락 시 자원 누수 위험 → `try-with-resources` 구문으로 개선 필요.
        

---

## 📌 요약

- 이 코드는 DB 테이블 `tbl_memo`에 메모를 저장하는 DAO 클래스.
    
- `DataSource`로 DB 연결, `PreparedStatement`로 안전하게 SQL 실행.
    
- `@Repository`를 통해 스프링 컨테이너에 자동 등록됨.
    
- 현재는 자원 해제 코드 누락 → `try-with-resources`로 개선 필요.