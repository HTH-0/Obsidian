# 🔌 MysqlDbUtils & Resource 설정 방식 변경 정리

---

## 📄 최신 MysqlDbUtils.java (Resource 기반)

```java
package Utils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;

public class MysqlDbUtils {
	
	private Connection conn;
	private PreparedStatement pstmt;
	private ResultSet rs;

	private DataSource dataSource;

	// 싱글톤
	private static MysqlDbUtils instance;

	private MysqlDbUtils() throws Exception {
		// JNDI로 데이터소스 가져오기
		Context initContext = new InitialContext();
		Context envContext  = (Context)initContext.lookup("java:/comp/env");
		dataSource = (DataSource)envContext.lookup("jdbc/MysqlDB");  
		conn = dataSource.getConnection();
		System.out.println("Connection : "+ conn);
	}

	public static MysqlDbUtils getInstance() throws Exception {
		if (instance == null) {
			instance = new MysqlDbUtils();
		}
		return instance;
	}

	public int insert(UserDto userDto) throws Exception {
		pstmt = conn.prepareStatement("INSERT INTO tbl_user VALUES (?, ?, ?)");
		pstmt.setString(1, userDto.getUsername());
		pstmt.setString(2, userDto.getPassword());
		pstmt.setString(3, userDto.getRole());
		int result = pstmt.executeUpdate();
		pstmt.close();
		return result;
	}

	public UserDto selectOne(String username) throws Exception {
		pstmt = conn.prepareStatement("SELECT * FROM tbl_user WHERE username = ?");
		pstmt.setString(1, username);
		rs = pstmt.executeQuery();

		UserDto userDto = null;
		if (rs != null && rs.next()) {
			userDto = new UserDto(rs.getString(1), rs.getString(2), rs.getString(3));
		}
		return userDto;
	}
}
```

---

## 📄 web.xml 내 리소스 설정

```xml
<resource-ref>
  <res-ref-name>jdbc/MysqlDB</res-ref-name>
  <res-type>javax.sql.DataSource</res-type>
  <res-auth>Container</res-auth>
</resource-ref>
```

---

## 🧠 코드 설명

### ✅ MysqlDbUtils 변경 사항 핵심 요약

- ✅ 기존 방식: `DriverManager.getConnection()` 직접 호출 → 하드코딩된 DB URL, ID, PW
    
- ✅ 개선 방식: **JNDI 리소스 기반으로 DB 연결**
    
    - `javax.naming.InitialContext`를 통해 서버 내 DataSource 조회
        
    - `conn = dataSource.getConnection()`으로 커넥션 풀 활용 가능
        

### ✅ 장점

- 서버의 DB 설정 정보와 애플리케이션 로직 분리됨 (유지보수 용이)
    
- 커넥션 풀 사용으로 성능 향상 (WAS 설정 필요)
    
- 보안상 유리 (소스코드에 ID/PW 노출 없음)
    

### ✅ web.xml 설정 설명

- `<resource-ref>`는 `MysqlDbUtils`에서 사용하는 JNDI 리소스를 정의
    
- `lookup("jdbc/MysqlDB")` 부분과 `res-ref-name`이 일치해야 함
    
- 실제 `context.xml`이나 톰캣 서버 설정에서 이 리소스 정의 필요
    

---

## ✅ 추가 팁: context.xml 예시 (Tomcat)

```xml
<Context>
  <Resource name="jdbc/MysqlDB"
            auth="Container"
            type="javax.sql.DataSource"
            maxTotal="20"
            maxIdle="10"
            driverClassName="com.mysql.cj.jdbc.Driver"
            url="jdbc:mysql://localhost:3306/testDB"
            username="root"
            password="1234"/>
</Context>
```

---

MysqlDbUtils`가 **JNDI 리소스 기반 구조**로 전환되어 있고,  
`web.xml`과 WAS 설정(`context.xml`)의 연결을 통해 **안정적인 커넥션 관리 구조**를 갖추고 있음.

---
