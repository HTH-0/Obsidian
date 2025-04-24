# 🧪 DataSource 및 DAO 연동 테스트 클래스

## 📦 전체 코드

```java
package DBTest;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.time.LocalDateTime;

import javax.sql.DataSource;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import com.example.app.domain.Dao.MemoDaoImpl;
import com.example.app.domain.Dto.MemoDto;

@ExtendWith(SpringExtension.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
class DataSourceTests {
	
	@Autowired
	private DataSource dataSource1;
	
	@Autowired
	private MemoDaoImpl memoDaoImpl;
	
	@Test
	void test1() throws Exception {
		System.out.println(dataSource1);
		Connection con = dataSource1.getConnection();
		PreparedStatement pstmt = con.prepareStatement("INSERT INTO tbl_book VALUES('abcd','abcd','abcd','abcd')");
		ResultSet rs;
		pstmt.executeUpdate();
	}
	
	@Test
	void test2() throws Exception {
		memoDaoImpl.insert(new MemoDto(1, "a", "a", LocalDateTime.now(), null));
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@ExtendWith(SpringExtension.class)`:
    
    - JUnit 5에서 스프링 테스트 기능을 확장함.
        
- `@ContextConfiguration(...)`:
    
    - 테스트에서 사용할 스프링 설정 파일 위치 지정.
        
    - `root-context.xml`을 기반으로 컨텍스트 초기화.
        

### ✅ 주요 필드 설명

- `@Autowired DataSource dataSource1`:
    
    - `root-context.xml`에서 정의한 `dataSource1` 빈을 주입받음.
        
- `@Autowired MemoDaoImpl memoDaoImpl`:
    
    - DAO 객체를 주입받아 메모 저장 기능 테스트 가능.
        

### ✅ 주요 메서드 설명

#### 🧪 `test1()`

- `DataSource`로부터 DB 커넥션을 얻고, SQL 직접 실행.
    
- `tbl_book` 테이블에 테스트용 더미 데이터 insert.
    
- 단순 연결 테스트용이며, 커넥션 종료 처리는 빠져 있음 (`close()` 필요).
    

#### 🧪 `test2()`

- `MemoDaoImpl`의 `insert()` 메서드 호출 테스트.
    
- `MemoDto` 객체를 직접 생성하여 DB에 insert 요청.
    

---

## 📌 요약

- 이 테스트 클래스는 `DataSource`와 `MemoDaoImpl`의 DB 연동 여부를 검증하는 JUnit5 기반 단위 테스트.
    
- root-context.xml의 설정이 제대로 적용되고 있는지 확인하는 데 사용.
    
- 커넥션 종료(close) 처리 추가 시 더욱 안전한 코드가 됨 (`try-with-resources` 추천).