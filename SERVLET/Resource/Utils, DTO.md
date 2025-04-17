# 🗄️ 데이터베이스 유틸리티 및 DTO 정리

---

## 📄 MysqlDbUtils.java

```java
package Utils;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class MysqlDbUtils {
    private static MysqlDbUtils instance;
    private Connection connection;
    private String url = "jdbc:mysql://localhost:3306/your_database";
    private String username = "root";
    private String password = "root";

    private MysqlDbUtils() throws Exception {
        // MySQL 드라이버 로드
        Class.forName("com.mysql.jdbc.Driver");
        // 데이터베이스 연결 생성
        this.connection = DriverManager.getConnection(url, username, password);
    }
    
    public static MysqlDbUtils getInstance() throws Exception {
        if(instance == null) {
            instance = new MysqlDbUtils();
        }
        return instance;
    }
    
    public int insert(UserDto user) throws Exception {
        String sql = "INSERT INTO tbl_user(username, password, role) VALUES (?, ?, ?)";
        PreparedStatement pstmt = connection.prepareStatement(sql);
        pstmt.setString(1, user.getUsername());
        pstmt.setString(2, user.getPassword());
        pstmt.setString(3, user.getRole());
        return pstmt.executeUpdate();
    }
    
    public UserDto selectOne(String username) throws Exception {
        String sql = "SELECT username, password, role FROM tbl_user WHERE username = ?";
        PreparedStatement pstmt = connection.prepareStatement(sql);
        pstmt.setString(1, username);
        ResultSet rs = pstmt.executeQuery();
        if(rs.next()){
            return new UserDto(rs.getString("username"), rs.getString("password"), rs.getString("role"));
        }
        return null;
    }
}
```

### 🧠 MysqlDbUtils 코드 설명

- **싱글톤 패턴 구현**:
    
    - `private static MysqlDbUtils instance;`와 `getInstance()` 메서드로 하나의 인스턴스만 생성하여 재사용함
        
    - 이를 통해 DB 커넥션을 효율적으로 관리 가능
        
- **생성자**:
    
    - `private MysqlDbUtils()`에서 MySQL 드라이버(`com.mysql.jdbc.Driver`)를 로드 후 `DriverManager.getConnection()`으로 DB 연결 생성
        
    - 접속 URL, 사용자명, 비밀번호는 실제 DB 환경에 맞게 수정
        
- **insert(UserDto user)**:
    
    - 전달받은 `UserDto` 객체의 데이터를 `tbl_user` 테이블에 삽입
        
    - `PreparedStatement`를 사용하여 SQL Injection 방지
        
- **selectOne(String username)**:
    
    - username 기준으로 사용자 정보를 조회하여 `UserDto` 객체로 리턴
        
    - 조회 결과가 없으면 `null` 반환
        

---

## 📄 OracleDBUtils.java

```java
package Utils;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

public class OracleDBUtils {
    private static OracleDBUtils instance;
    private Connection connection;
    private String url = "jdbc:oracle:thin:@localhost:1521:XE";
    private String username = "oracle";
    private String password = "oracle";

    private OracleDBUtils() throws Exception {
        // Oracle 드라이버 로드
        Class.forName("oracle.jdbc.driver.OracleDriver");
        // 데이터베이스 연결 생성
        this.connection = DriverManager.getConnection(url, username, password);
    }
    
    public static OracleDBUtils getInstance() throws Exception {
        if(instance == null) {
            instance = new OracleDBUtils();
        }
        return instance;
    }
    
    public int insert(UserDto user) throws Exception {
        String sql = "INSERT INTO tbl_user(username, password, role) VALUES (?, ?, ?)";
        PreparedStatement pstmt = connection.prepareStatement(sql);
        pstmt.setString(1, user.getUsername());
        pstmt.setString(2, user.getPassword());
        pstmt.setString(3, user.getRole());
        return pstmt.executeUpdate();
    }
    
    public UserDto selectOne(String username) throws Exception {
        String sql = "SELECT username, password, role FROM tbl_user WHERE username = ?";
        PreparedStatement pstmt = connection.prepareStatement(sql);
        pstmt.setString(1, username);
        ResultSet rs = pstmt.executeQuery();
        if(rs.next()){
            return new UserDto(rs.getString("username"), rs.getString("password"), rs.getString("role"));
        }
        return null;
    }
}
```

### 🧠 OracleDBUtils 코드 설명

- **Oracle 전용 유틸리티**:
    
    - MySQL과 유사한 구조지만, Oracle 데이터베이스에 맞는 드라이버와 URL 사용
        
    - `Class.forName("oracle.jdbc.driver.OracleDriver")`를 통해 Oracle 드라이버 로드
        
- **싱글톤 패턴**:
    
    - `getInstance()` 메서드로 단일 인스턴스 생성 및 재사용하여 DB 커넥션을 관리함
        
- **데이터 삽입/조회**:
    
    - `insert(UserDto user)`와 `selectOne(String username)` 메서드로 각각 데이터를 삽입하고 조회
        
    - SQL 구문은 Oracle 환경에 맞추어 작성됨
        

---

## 📄 UserDto.java

```java
package Utils;

public class UserDto {
    private String username;
    private String password;
    private String role;
    
    public UserDto(String username, String password, String role) {
        this.username = username;
        this.password = password;
        this.role = role;
    }

    public String getUsername() {
        return username;
    }
    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }

    public String getRole() {
        return role;
    }
    public void setRole(String role) {
        this.role = role;
    }
}
```

### 🧠 UserDto 코드 설명

- **데이터 전송 객체(DTO)**:
    
    - `UserDto` 클래스는 사용자 정보를 담는 간단한 객체로, DB와 애플리케이션 간 데이터 전달에 사용됨
        
    - 주요 필드: `username`, `password`, `role`
        
- **생성자 및 Getter/Setter**:
    
    - 생성자를 통해 초기값 설정하며, `getUsername()`, `getPassword()`, `getRole()` 등의 메서드로 값 반환
        
    - `setUsername()`, `setPassword()`, `setRole()`를 통해 값 변경 가능
        

---

이와 같이, **MysqlDbUtils**와 **OracleDBUtils**는 각각의 데이터베이스에 연결하여 데이터를 조작하는 유틸리티 클래스로,  
**UserDto**는 사용자 데이터를 저장하는 역할을 수행합니다.  
필요에 따라 실제 데이터베이스 정보 및 SQL 구문을 환경에 맞게 수정하여 사용할 수 있습니다.