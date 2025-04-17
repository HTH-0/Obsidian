
# 🗄️ 10번 프로젝트 DAO 계층 전체 정리

---

## 1️⃣ Dao.java (공통 추상 클래스)

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
		connectionPool = ConnectionPool.getInstance();
	}
}
```

### 🧠 설명

- 모든 DAO 클래스의 **공통 기반 클래스**
    
- `connectionPool`과 `connectionItem`, `pstmt`, `rs`를 멤버로 보유
    
- 생성자에서 커넥션 풀 인스턴스를 초기화함
    
- `UserDaoImpl`, `BookDaoImpl` 등에서 이 클래스를 상속
    

---

## 2️⃣ UserDao.java (인터페이스)

```java
package Domain.Dao;

import java.sql.SQLException;
import java.util.List;
import Domain.Dto.UserDto;

public interface UserDao {
	int insert(UserDto userDto) throws Exception;
	int update(UserDto userDto) throws SQLException;
	int delete(UserDto userDto) throws SQLException;

	// 단건 조회
	UserDto select(String username) throws Exception;

	// 다건 조회
	List<UserDto> selectAll();
}
```

### 🧠 설명

- 사용자 관련 DB 작업을 정의한 인터페이스
    
- `insert`, `update`, `delete`, `select`, `selectAll` 메서드 포함
    
- 실 구현체는 `UserDaoImpl`
    

---

## 3️⃣ UserDaoImpl.java

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
			try { pstmt.close(); } catch (Exception e2) {}
		}
	}

	@Override
	public int update(UserDto userDto) throws SQLException {
		return 0;
	}

	@Override
	public int delete(UserDto userDto) throws SQLException {
		return 0;
	}

	@Override
	public UserDto select(String username) throws Exception {
		try {
			connectionItem = connectionPool.getConnection();
			Connection conn = connectionItem.getConn();

			pstmt = conn.prepareStatement("select * from tbl_user where username=?");
			pstmt.setString(1, username);
			rs = pstmt.executeQuery();

			UserDto userDto = null;
			if (rs != null && rs.next())
				userDto = new UserDto(rs.getString(1), rs.getString(2), rs.getString(3));

			connectionPool.releaseConnection(connectionItem);
			return userDto;

		} catch (SQLException e) {
			e.printStackTrace();
			throw new SQLException("USERDAO's SELECT SQL EXCEPTION!!");
		} finally {
			try { pstmt.close(); } catch (Exception e2) {}
		}
	}

	@Override
	public List<UserDto> selectAll() {
		return null;
	}
}
```

### 🧠 설명

- `Dao` 추상 클래스를 상속하고 `UserDao` 인터페이스 구현
    
- 현재 `insert()`와 `select(username)`만 구현되어 있음
    
- `ConnectionPool`을 이용해 커넥션을 얻고, 사용 후 반납하는 구조
    
- `ROLE_USER`로 고정하여 가입 처리
    

---

## ✅ 전체 흐름 요약

```
[컨트롤러]
  ↓
[UserServiceImpl.userJoin() 또는 login()]
  ↓
[UserDaoImpl.insert() / select()]
  ↓
[커넥션 풀에서 커넥션 획득 → SQL 실행 → 반납]
```

---

