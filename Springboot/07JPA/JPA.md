
---

# ✅ SpringBoot JPA 개념 및 실습 정리

## 📘 1. JPA 핵심 개념 정리

### 📌 주요 애노테이션

|애노테이션|설명|
|---|---|
|`@Entity`|JPA 엔티티로 지정 (DB 테이블과 매핑됨)|
|`@Table(name="...")`|엔티티가 매핑될 테이블 이름 지정|
|`@Id`|엔티티의 기본 키(PK) 필드 지정|
|`@GeneratedValue(strategy=...)`|PK 자동 생성 전략 설정|
|`@Column`|필드 ↔ 컬럼 매핑 및 속성 지정 (길이, nullable 등)|
|`@Temporal`|날짜 필드 타입 지정 (DATE, TIME, TIMESTAMP)|
|`@OneToMany`, `@ManyToOne`, `@OneToOne`, `@ManyToMany`|관계 매핑용 애노테이션|
|`@JoinColumn`|외래 키 매핑 지정|
|`@JoinTable`|다대다 중간 테이블 지정|
|`@Transient`|DB에 매핑되지 않는 필드|
|`@PrimaryKeyJoinColumn`|상속 매핑 시 PK 공유 지정|

---

## 🛠 2. 실습 설정 요약

### 📦 build.gradle

```groovy
implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
runtimeOnly 'com.mysql:mysql-connector-j'
```

### 📦 application.properties

```properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.jdbc.batch_size=1000
spring.jpa.properties.hibernate.order_inserts=true
spring.jpa.properties.hibernate.order_updates=true
spring.jpa.properties.hibernate.jdbc.batch_versioned_data=true
spring.sql.init.mode=always
spring.jpa.defer-datasource-initialization=true
```

---

## ⚙ 3. JPA 설정 Java 파일

### 📂 `JpaConfig.java`

- `@EnableJpaRepositories`: Repository 패키지 위치 등록
    
- `@EntityScan`: Entity 패키지 위치 등록
    
- schema.sql, data.sql 자동 실행 구성 포함
    

---

## 🧩 4. Entity 구성

### 📘 Book.java

```java
@Entity
@Table(name="book")
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="bookcode")
    private Long bookCode;
    
    @Column(name="bookname", length=1024)
    private String bookName;
    
    private String publisher;
    private String author;
    private String isbn;
}
```

### 📘 User.java

```java
@Entity
public class User {
    @Id
    @Column(length=100)
    private String username;

    @Column(nullable=false)
    private String password;

    private String role;
}
```

---

## 🗃 5. Repository 구성

### 📂 BookRepository.java

```java
@Repository
public interface BookRepository extends JpaRepository<Book, Long> {
    List<Book> findByBookname(String bookname);
    List<Book> findByPublisher(String publisher);
    Book findByIsbn(String isbn);
    List<Book> findByBookcodeBetween(long start, long end);
    List<Book> findByBooknameOrPublisher(String bookname, String publisher);
    List<Book> findByBooknameOrPublisherOrderByBooknameAsc(String bookname, String publisher);
    List<Book> findByBooknameContaining(String keyword);
    List<Book> findByPublisherStartingWith(String prefix);
    int countByBookname(String bookname);
    int countByPublisher(String publisher);
    void deleteByBookname(String bookname);
}
```

### 📂 UserRepository.java

```java
@Repository
public interface UserRepository extends JpaRepository<User, String> {
}
```

---

## 🧪 6. 테스트 코드 예시

### 📂 BookRepositoryTest.java

```java
@SpringBootTest
class BookRepositoryTest {
    @Autowired
    private BookRepository bookRepository;

    @Test
    public void t1() {
        Book book = Book.builder().bookCode(1L)...build();
        Book result = bookRepository.save(book);
    }

    @Test
    public void t2() {
        bookRepository.deleteById(1L);
    }

    @Test
    public void t4() {
        Optional<Book> book = bookRepository.findById(1L);
    }

    @Test
    public void t5() {
        List<Book> list = bookRepository.findAll();
        list.forEach(System.out::println);
    }
}
```

---

## 📌 요약

- `@Entity`, `@Id`, `@GeneratedValue`, `@Column` 등 기본 애노테이션 숙지
    
- `JpaRepository` 인터페이스를 통해 CRUD 및 쿼리 메소드 구현 가능
    
- Spring Boot에서 `ddl-auto=update` 설정을 통해 DB 자동 생성 가능
    
- `schema.sql`, `data.sql` 초기화도 별도 지원 가능 (`DataSourceInitializer`)
    
- 테스트는 `@SpringBootTest`로 통합 환경에서 동작
    

---
