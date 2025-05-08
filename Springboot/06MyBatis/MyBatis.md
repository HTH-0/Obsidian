
---

# ✅ Spring Boot + MyBatis + DataSource 전체 구조 정리

---

## 1️⃣ DataSource 설정

### 📦 `build.gradle`

```groovy
implementation 'org.springframework.boot:spring-boot-starter-jdbc'
runtimeOnly 'com.mysql:mysql-connector-j'
implementation group: 'org.apache.commons', name: 'commons-dbcp2', version: '2.12.0'
```

---

### 📦 `DataSourceConfig.java`

```java
@Bean
public DataSource dataSource3() {
    HikariDataSource dataSource = new HikariDataSource();
    dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
    dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/bookdb");
    dataSource.setUsername("root");
    dataSource.setPassword("1234");
    return dataSource;
}
```

---

## 2️⃣ MyBatis 설정

### 📦 `MybatisConfig.java`

```java
@Autowired
private DataSource dataSource3; // 위에서 설정한 HikariDataSource

@Bean
public SqlSessionFactory sqlSessionFactory() throws Exception {
    SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
    sessionFactory.setDataSource(dataSource3);
    Resource[] resources = new PathMatchingResourcePatternResolver()
            .getResources("classpath*:mapper/*.xml");
    sessionFactory.setMapperLocations(resources);
    return sessionFactory.getObject();
}

@Bean
public SqlSessionTemplate sqlSessionTemplate() throws Exception {
    return new SqlSessionTemplate(sqlSessionFactory());
}
```

---

## 3️⃣ Mapper 및 XML 구성

### 📂 `MemoMapper.java` (Interface)

→ 파일은 누락되었지만 추정 구조:

```java
public interface MemoMapper {
    void insert(MemoDto dto);
    void insertXml(MemoDto dto);
    ...
}
```

---

### 📂 `MemoMapper.xml`

```xml
<insert id="insertXml" useGeneratedKeys="true" keyProperty="id">
    <selectKey keyProperty="id" order="AFTER" resultType="int">
        select max(id) + 1 as id from tbl_memo
    </selectKey>
    insert into tbl_memo (id, text) values (#{id}, #{text})
</insert>

<update id="updateXml">
    update tbl_memo set text=#{text} where id=#{id}
</update>

<delete id="deleteXml">
    delete from tbl_memo where id=#{id}
</delete>

<select id="selectAtXml" resultType="com.example.demo.domain.dto.MemoDto" parameterType="int">
    select * from tbl_memo where id=#{id}
</select>
```

---

## 4️⃣ 테스트 코드 구성

### 📂 `MemoMapperTest.java`

```java
@Test
public void t1() {
    MemoDto memoDto = new MemoDto(444,"a","a", LocalDateTime.now(),null);
    memoMapper.insert(memoDto); // @Insert 어노테이션 기반
}

@Test
public void t2() {
    MemoDto memoDto = new MemoDto(555,"a","a", LocalDateTime.now(),null);
    memoMapper.insertXml(memoDto); // XML 기반 insert
}
```

---

### 📂 `DataSourceConfigTest.java`

직접 커넥션 열어서 JDBC 테스트

```java
@Autowired
private DataSource dataSource3;

@Test
public void t2() {
    Connection conn = dataSource3.getConnection();
    ...
    pstmt.executeUpdate();
}
```

---

### 📂 `MybatisConfigTest.java`

SqlSessionFactory와 SqlSession 객체 정상 생성 확인

```java
@Autowired
private SqlSessionFactory sqlSessionFactory;

@Test
public void t1() {
    SqlSession sqlSession = sqlSessionFactory.openSession();
    ...
}
```

---

## 5️⃣ tbl_memo 테이블 구조 예상

XML과 테스트 코드를 기준으로 최소한 다음 컬럼들이 필요:

```sql
CREATE TABLE tbl_memo (
    id INT PRIMARY KEY,
    text VARCHAR(100),
    writer VARCHAR(100),
    email VARCHAR(100),
    createAt DATETIME
);
```

> ✅ `insertXml`에서는 `text`만 넣고 있음  
> ✅ `insert()`에서는 `id, text, writer, email, createAt` 다 사용

---

## ✅ 정리 요약

|구성 요소|설명|
|---|---|
|DataSource 설정|`@Bean`으로 직접 HikariCP 등록 (dataSource3)|
|MyBatis 설정|`MybatisConfig.java`에서 SqlSessionFactory 수동 구성|
|Mapper 연결|Java 인터페이스 + XML Mapper (`MemoMapper.xml`)|
|테스트 방식|1. 순수 JDBC (`DataSourceConfigTest`) 2. MyBatis 테스트 (`MemoMapperTest`)|
|테이블 구조|`tbl_memo(id, text, writer, email, createAt)` 로 구성 예상|

---
