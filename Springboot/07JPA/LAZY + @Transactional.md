
---

# ✅ FetchType.LAZY + @Transactional 왜 같이 써야 할까?

---

## 📘 1. 지연 로딩(LAZY)이란?

- `@ManyToOne(fetch = FetchType.LAZY)` 또는 `@OneToMany(fetch = LAZY)` 설정 시
    
- 연관 객체를 **즉시 가져오는 게 아니라**, 실제 해당 필드를 **접근할 때 쿼리 실행됨**
    

```java
User user = lend.getUser(); // 이 순간 쿼리 발생
```

---

## 📗 2. 왜 문제가 발생할까?

### ❌ 문제 상황 (지연 로딩 + 트랜잭션 없음)

```java
@Test
public void t1() {
    Optional<Lend> lend = lendRepository.findById(1L);
    System.out.println(lend.get().getUser().getUsername()); // 에러 발생 가능
}
```

- 이 테스트는 `@Transactional`이 없음
    
- Spring이 DB 세션을 닫은 뒤에 `.getUser()` 호출 → **LazyInitializationException** 발생
    

---

## ✅ 해결 방법: `@Transactional` 사용

```java
@Test
@Transactional
public void t2() {
    Optional<Lend> lend = lendRepository.findById(1L);
    System.out.println(lend.get().getUser().getUsername()); // OK
}
```

- `@Transactional`이 있으면 메서드 전체에 트랜잭션이 열려 있음
    
- 영속성 컨텍스트도 살아 있어서 `.getUser()` 호출 시 LAZY 쿼리 정상 실행됨
    

---

## 🧠 핵심 요약

|항목|설명|
|---|---|
|`FetchType.LAZY`|연관 객체를 실제로 사용할 때 쿼리 실행|
|문제 원인|지연 로딩 시점에 영속성 컨텍스트(session)가 이미 닫혀 있으면 에러|
|해결 방법|`@Transactional`을 붙여서 트랜잭션 범위 내에서 연관 객체에 접근|

---

## 🧪 실제 예시 (LendRepositoryTest)

```java
@Test
@Transactional
public void t5() throws Exception {
    Optional<Lend> lendOptional = lendRepository.findById(2L);
    System.out.println("findById 실행됨");
    Lend lend = lendOptional.get();

    System.out.println("user.get 호출 전");
    User user = lend.getUser(); // 이 시점에서 쿼리 실행
    System.out.println(user);
}
```

> ✅ LAZY 객체(`user`)는 메서드 안에서만 접근해야 함  
> ✅ 그 범위를 벗어나면 Hibernate가 세션을 닫아버려서 **LazyInitializationException** 발생

---

## 📌 요약

- **LAZY는 지연 실행** → 객체 접근 시점까지 기다림
    
- **세션이 닫히면 LAZY 실패** → `@Transactional`로 보호 필요
    
- 테스트 코드나 서비스 로직에서 **LAZY 필드 접근 = 트랜잭션 열려 있어야 함**
    

---
