
---

# ✅ JPA 양방향 매핑 - Book → Lend (`@OneToMany`)

---

## 📘 1. 목표

- 기존 `Lend.java`는 `@ManyToOne(fetch = EAGER)`로 `Book`을 참조하고 있음
    
- 이제 `Book.java`에서도 여러 개의 `Lend`를 조회할 수 있도록 **양방향 관계** 설정
    

---

## 📗 2. Book.java 수정

### ✅ 변경 전

```java
@Entity
@Table(name="book")
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="bookcode")
    private Long bookCode;

    @Column(name="bookname" ,length = 1024)
    private String bookName;

    private String publisher;
    private String author;
    private String isbn;
}
```

---

### ✅ 변경 후

```java
@OneToMany(mappedBy = "book", cascade = CascadeType.ALL, orphanRemoval = true)
private List<Lend> lends = new ArrayList<>();
```

```java
@Entity
@Table(name="book")
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name="bookcode")
    private Long bookCode;

    @Column(name="bookname", length = 1024)
    private String bookName;

    private String publisher;
    private String author;
    private String isbn;

    @OneToMany(mappedBy = "book", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Lend> lends = new ArrayList<>();
}
```

> ✅ `mappedBy = "book"`은 Lend 엔티티의 `book` 필드와 연결  
> ✅ `List<Lend>`를 통해 책과 연결된 대출 이력 모두 조회 가능

---

## 🧪 3. 테스트 코드 예시

```java
@Test
@Transactional
public void testBookToLend양방향조회() {
    Book book = bookRepository.findById(333L).get();
    List<Lend> lends = book.getLends(); // 대출 이력 조회
    lends.forEach(System.out::println);
}
```

---

## 🛠 양방향 매핑 실습 요약

|주체|대상|주인 관계|컬렉션 필드명|
|---|---|---|---|
|User|Lend|Lend.user|User.lends|
|Book|Lend|Lend.book|Book.lends|

---

## 📌 정리

- `@OneToMany(mappedBy = "...")`는 연관관계의 주인이 아님
    
- 주인은 항상 외래키가 있는 쪽 (`@ManyToOne`)
    
- LAZY 전략일 경우 컬렉션 접근 시점에 쿼리 실행됨
    
- 양방향 설정으로 조회 편의성 향상 (user → lend, book → lend)
    

---
