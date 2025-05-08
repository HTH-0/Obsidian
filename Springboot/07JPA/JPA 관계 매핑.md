
---

# ✅ JPA 관계 매핑 실습 - Lend (대출 이력 테이블)

---

## 📘 1. Lend 엔티티 구조

### 📂 `Lend.java`

```java
@Entity
@Table(name = "lend")
@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class Lend {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Book: 다대일 (N:1)
    @ManyToOne(fetch = FetchType.EAGER) // 기본값 EAGER
    @JoinColumn(name = "bookcode", foreignKey = @ForeignKey(
        name = "FK_LEND_BOOK",
        foreignKeyDefinition = "FOREIGN KEY(bookcode) REFERENCES book(bookcode) ON DELETE CASCADE ON UPDATE CASCADE"
    ))
    private Book book;

    // User: 다대일 (N:1)
    @ManyToOne(fetch = FetchType.LAZY) // LAZY 설정
    @JoinColumn(name = "username", foreignKey = @ForeignKey(
        name = "FK_LEND_USER",
        foreignKeyDefinition = "FOREIGN KEY(username) REFERENCES user(username) ON DELETE CASCADE ON UPDATE CASCADE"
    ))
    private User user;
}
```

> ✅ 즉시 로딩(EAGER)과 지연 로딩(LAZY) 모두 사용해 비교 가능하게 구성  
> ✅ 외래 키 `@JoinColumn` 명시적 지정 + `@ForeignKey` 제약 조건 이름 설정

---

## 📗 2. LendRepository 쿼리 메서드

### 📂 `LendRepository.java`

```java
@Repository
public interface LendRepository extends JpaRepository<Lend, Long> {

    @Query("SELECT l FROM Lend AS l JOIN FETCH l.user WHERE l.user.username = :username")
    List<Lend> findLendsByUser(@Param("username") String username);

    @Query("SELECT l FROM Lend AS l JOIN FETCH l.user JOIN FETCH l.book")
    List<Lend> findLendsByUserAndBook();
}
```

> ✅ `JOIN FETCH` 를 통해 지연 로딩되는 객체를 한 번에 조회  
> ✅ `findLendsByUser()` 는 유저 이름 기준으로 대출 이력 가져옴

---

## 🧪 3. LendRepository 테스트 코드

### 📂 `LendRepositoryTest.java`

```java
@SpringBootTest
class LendRepositoryTest {

    @Autowired
    private LendRepository lendRepository;
    @Autowired
    private UserRepository userRepository;
    @Autowired
    private BookRepository bookRepository;

    @Test
    public void t1() {
        // 도서, 사용자 조회 → 대출 등록
        Book book = bookRepository.findById(333L).get();
        User user = userRepository.findById("user2").get();

        Lend lend = new Lend();
        lend.setBook(book);
        lend.setUser(user);
        lendRepository.save(lend);
    }

    @Test
    public void t2() {
        // 대출된 도서 변경
        Lend lend = lendRepository.findById(1L).get();
        Book book = bookRepository.findById(444L).get();
        lend.setBook(book);
        lendRepository.save(lend);
    }

    @Test
    public void t3() {
        // 삭제
        lendRepository.deleteById(1L);
    }

    @Test
    public void t4() {
        // 특정 유저의 대출 목록 조회
        List<Lend> list = lendRepository.findLendsByUser("user1");
        list.forEach(System.out::println);
    }

    @Test
    @Transactional
    public void t5() {
        // LAZY 테스트: getUser() 시점에 쿼리 실행
        Optional<Lend> lendOptional = lendRepository.findById(2L);
        Lend lend = lendOptional.get();

        // 지연 로딩된 user 엔티티를 접근하는 순간 쿼리 실행
        User user = lend.getUser(); 
        System.out.println(user);
    }
}
```

---

## 🔍 EAGER vs LAZY 요약

|전략|설명|쿼리 실행 시점|
|---|---|---|
|`EAGER`|즉시 로딩. 연관 객체를 즉시 join하여 함께 조회|`.findById()` 시점|
|`LAZY`|지연 로딩. 실제로 객체 접근할 때 쿼리 실행|`.getUser()` 등 호출 시점|

---

## ✅ 최종 요약

- `Lend` 엔티티는 `Book` 및 `User`와 다대일 관계를 가짐 (`@ManyToOne`)
    
- 외래 키는 `@JoinColumn`으로 지정, `@ForeignKey` 설정 포함
    
- `EAGER`, `LAZY` 전략 모두 테스트 가능하게 구성
    
- Repository에서는 `@Query` + `JOIN FETCH` 조합으로 최적화된 조회
    
- 테스트 코드에서 저장, 수정, 삭제, 조회, 지연 로딩 확인 가능
    

---
