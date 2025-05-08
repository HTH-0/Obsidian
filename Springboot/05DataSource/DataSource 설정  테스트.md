# ✅ SpringBoot DataSource 설정 및 테스트 정리

---

## 1️⃣ 의존성 및 설정 파일 구성

### 📦 build.gradle

```groovy
// JDBC
implementation 'org.springframework.boot:spring-boot-starter-jdbc'
runtimeOnly 'com.mysql:mysql-connector-j'

// 커스텀 커넥션 풀: Apache Commons DBCP2
implementation group: 'org.apache.commons', name: 'commons-dbcp2', version: '2.12.0'

// (기본 포함되어 있음) HikariCP는 spring-boot-starter-jdbc 내부에 이미 포함됨
```

---

### 📦 application.properties

```properties
# DataSource 기본 설정
spring.datasource.url=jdbc:mysql://localhost:3306/testdb
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=1234
```

📝 주의: `application.properties`는 `@SpringBootTest` 환경에서 기본 DataSource에 사용되며, Java 설정 클래스와 함께 사용할 때는 직접 Bean으로 등록한 것을 테스트에서 불러오는 방식 사용 가능

---

## 2️⃣ Java 설정 기반 커넥션 풀 구성

### 📂 `DataSourceConfig.java`

```java
@Configuration
public class DataSourceConfig {

    // Apache Commons DBCP2
    @Bean
    public DataSource dataSource2() {
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/bookdb");
        dataSource.setUsername("root");
        dataSource.setPassword("1234");

        dataSource.setInitialSize(5);
        dataSource.setMaxTotal(10);
        dataSource.setMaxIdle(8);
        dataSource.setMinIdle(3);

        return dataSource;
    }

    // HikariCP
    @Bean
    public HikariDataSource dataSource3() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/bookdb");
        dataSource.setUsername("root");
        dataSource.setPassword("1234");
        return dataSource;
    }
}
```

---

## 3️⃣ 테스트 코드 구성

### 📂 `DemoApplicationTests.java`

```java
@SpringBootTest
class DemoApplicationTests {
    @Test
    void contextLoads() {
        // context가 정상적으로 로드되는지만 확인
    }
}
```

---

### 📂 `DataSourceTests.java`

(application.properties에 설정된 기본 DataSource 테스트)

```java
@SpringBootTest
public class DataSourceTests {

    @Autowired
    private DataSource dataSource;

    @Test
    public void t1() throws Exception {
        String sql = "INSERT INTO tbl_memo VALUES (?,?,?,now());";
        Connection conn = dataSource.getConnection();
        PreparedStatement pstmt = conn.prepareStatement(sql);

        pstmt.setInt(1, 111);
        pstmt.setString(2, "aaabbb");
        pstmt.setString(3, "springboot@test.com");

        pstmt.executeUpdate();
    }
}
```

---

### 📂 `DataSourceConfigTest.java`

`@Bean`으로 등록한 dataSource2 (DBCP2)와 dataSource3 (HikariCP) 각각 테스트

```java
@SpringBootTest
class DataSourceConfigTest {

    @Autowired
    private DataSource dataSource2;

    @Autowired
    private DataSource dataSource3;

    @Test
    public void t1() throws Exception {
        String sql = "INSERT INTO tbl_memo VALUES (?,?,?,now());";
        Connection conn = dataSource2.getConnection();
        PreparedStatement pstmt = conn.prepareStatement(sql);

        pstmt.setInt(1, 222);
        pstmt.setString(2, "aaaabbb");
        pstmt.setString(3, "springboot@test.com");

        pstmt.executeUpdate();
    }

    @Test
    public void t2() throws Exception {
        String sql = "INSERT INTO tbl_memo VALUES (?,?,?,now());";
        Connection conn = dataSource3.getConnection();
        PreparedStatement pstmt = conn.prepareStatement(sql);

        pstmt.setInt(1, 333);
        pstmt.setString(2, "aaaabbb");
        pstmt.setString(3, "springboot@test.com");

        pstmt.executeUpdate();
    }
}
```

---

## ✅ 요약

|구분|설명|
|---|---|
|설정 방법|`application.properties`, Java `@Bean` 기반 수동 등록 병행|
|커넥션 풀 종류|DBCP2 (`dataSource2()`), HikariCP (`dataSource3()`)|
|테스트 대상|`DataSourceTests`: application.properties 사용 `DataSourceConfigTest`: 수동 등록한 Bean 사용|
|JDBC 연결 테스트|`tbl_memo` 테이블에 insert 문 수행하여 정상 연결 여부 확인|
