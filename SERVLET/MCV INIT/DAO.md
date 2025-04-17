# 🗃️ Dao 계층 구조 및 UserDaoImpl 기능 정리 

---

## 📄 Dao.java (공통 추상 클래스)

```java
package Domain.Dao;

import java.sql.PreparedStatement;
import java.sql.ResultSet;
import Domain.Dao.ConnectionPool.ConnectionItem;
import Domain.Dao.ConnectionPool.ConnectionPool;

public abstract class Dao {

	protected PreparedStatement pstmt;
	protected ResultSet rs;

	protected ConnectionPool connectionPool;
	protected ConnectionItem connectionItem;

	public Dao() throws Exception {
		// 커넥션 풀 초기화
		connectionPool = ConnectionPool.getInstance();
	}
}
```

### 🧠 설명

- 모든 DAO 클래스가 상속받는 추상 클래스
    
- `PreparedStatement`, `ResultSet`, `ConnectionPool`, `ConnectionItem` 공통 멤버 제공
    
- 생성자에서 커넥션 풀 초기화
    

---

## 📄 UserDao.java (인터페이스)

```java
package Domain.Dao;

import java.sql.SQLException;
import java.util.List;
import Domain.Dto.UserDto;

public interface UserDao {

	int insert(UserDto userDto) throws Exception;
	int update(UserDto userDto) throws SQLException;
	int delete(UserDto userDto) throws SQLException;
	UserDto select(UserDto userDto);
	List<UserDto> selectAll();
}
```

### 🧠 설명

- 사용자 정보를 다루는 DAO의 기능 정의
    
- CRUD 중심의 추상 메서드들 포함
    

---

## 📄 UserDaoImpl.java

```java
package Domain.Dao;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

import Domain.Dto.UserDto;

public class UserDaoImpl extends Dao implements UserDao {

	private static UserDao instance;

	private UserDaoImpl() throws Exception {
		System.out.println("[DAO] UserDaoImpl init...");
	}

	public static UserDao getInstance() throws Exception {
		if (instance == null)
			instance = new UserDaoImpl();
		return instance;
	}

	@Override
	public int insert(UserDto userDto) throws Exception {
		try {
			connectionItem = connectionPool.getConnection();
			Connection conn = connectionItem.getConn();

			pstmt = conn.prepareStatement("insert into tbl_user values(?,?,?)");
			pstmt.setString(1, userDto.getUsername());
			pstmt.setString(2, userDto.getPassword());
			pstmt.setString(3, "ROLE_USER");

			connectionPool.releaseConnection(connectionItem);
			return pstmt.executeUpdate();

		} catch (SQLException e) {
			e.printStackTrace();
			throw new SQLException("USERDAO's INSERT SQL EXCEPTION!!");
		} finally {
			try {
				pstmt.close();
			} catch (Exception e2) {
			}
		}
	}

	@Override public int update(UserDto userDto) { return 0; }
	@Override public int delete(UserDto userDto) { return 0; }
	@Override public UserDto select(UserDto userDto) { return null; }
	@Override public List<UserDto> selectAll() { return null; }
}
```

---

## ✅ 핵심 요약

|항목|설명|
|---|---|
|상속 구조|`UserDaoImpl` → `Dao` 추상 클래스 상속|
|패턴 적용|싱글톤 패턴으로 인스턴스 생성 제한|
|커넥션 관리|커스텀 커넥션 풀(`ConnectionPool`) 사용|
|주요 기능|현재 `insert()` 기능만 구현되어 있음 `update()`, `delete()`, `select()` 등은 추후 확장 예정|

---

## 🧩 연계 정보

- `UserCreateController`에서 `UserServiceImpl` 통해 `userDao.insert()` 호출
    
- `UserDaoImpl.insert()`는 `tbl_user` 테이블에 사용자 정보 저장
    

---
