
---

# ✅ @OrderColumn - @OneToMany 컬렉션 순서 유지

---

## 📘 1. 언제 사용하는가?

- `List`나 `ArrayList`를 사용하는 `@OneToMany` 컬렉션에서
    
- 데이터베이스에서 가져온 순서를 **항상 일관되게 유지하고 싶을 때**
    
- 단순 `ORDER BY`와 다르게 **별도의 정렬용 컬럼을 사용함**
    

---

## 📗 2. 기본 사용법

```java
@OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
@OrderColumn(name = "sort_order")
private List<Lend> lends = new ArrayList<>();
```

- `sort_order` 라는 컬럼이 테이블에 생성됨
    
- List 순서에 따라 자동으로 정렬 값(0부터 시작)이 저장됨
    
- 변경되면 그에 따라 순서도 자동 업데이트됨
    

---

## 🧩 3. 테이블 컬럼 예시

```sql
CREATE TABLE lend (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    bookcode BIGINT,
    username VARCHAR(100),
    sort_order INT, -- ← 순서 컬럼 자동 관리
    ...
);
```

---

## 🧪 4. 사용 예시

```java
@Test
@Transactional
public void testOrderColumn() {
    User user = userRepository.findById("user1").get();

    List<Lend> lends = user.getLends(); // 순서 보장
    for (Lend lend : lends) {
        System.out.println("순서: " + lends.indexOf(lend) + ", 도서: " + lend.getBook().getBookName());
    }
}
```

---

## ⚠️ 주의 사항

|항목|설명|
|---|---|
|컬렉션 타입|`List`, `ArrayList`에서만 적용 (Set 불가)|
|컬럼 이름|명시하지 않으면 기본 이름은 `lends_ORDER` 같이 자동 생성됨|
|순서 변경 시|내부적으로 update 쿼리로 정렬 순서 갱신됨 (성능 주의)|

---

## ✅ 요약

- `@OrderColumn`은 `List` 컬렉션의 순서를 DB 컬럼으로 유지시켜줌
    
- 별도 정렬 컬럼 (`sort_order`)이 DB에 생성됨
    
- 순서를 보장해야 할 경우에만 사용 → 트래픽 크면 주의 필요
    
- `ORDER BY`는 정렬만 해주고, `@OrderColumn`은 순서를 저장해줌
    

---
