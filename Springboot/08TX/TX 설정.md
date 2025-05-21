
---

# ✅ 트랜잭션 설정 (JDBC + JPA 동시 지원)

## 📦 전체 코드

### 🔧 TxConfig.java

```java
@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
    basePackages = "com.example.demo.domain.repository",
    transactionManagerRef = "jpaTransactionManager"
)
public class TxConfig {

    @Autowired
    private DataSource dataSource2;

    @Bean(name = "transactionManager")
    public DataSourceTransactionManager transactionManager2() {
        return new DataSourceTransactionManager(dataSource2);
    }

    @Bean(name = "jpaTransactionManager")
    public JpaTransactionManager jpaTransactionManager(EntityManagerFactory entityManagerFactory) {
        JpaTransactionManager transactionManager = new JpaTransactionManager();
        transactionManager.setEntityManagerFactory(entityManagerFactory);
        transactionManager.setDataSource(dataSource2);
        return transactionManager;
    }
}
```

---

### 🧪 TxTestService.java

```java
@Service
@Slf4j
public class TxTestService {

    @Autowired
    private MemoMapper memoMapper;

    @Transactional(rollbackFor = SQLException.class, transactionManager = "transactionManager")
    public void addMemoTx(MemoDto dto) throws Exception {
        log.info("MemoService's addMemoTx Call!");
        memoMapper.insert(dto);
        throw new SQLException(); // 강제 롤백 유도
    }

    @Autowired
    private MemoRepository memoRepository;

    @Transactional(rollbackFor = SQLException.class, transactionManager = "jpaTransactionManager")
    public void addMemoTx2(MemoDto dto) throws Exception {
        log.info("MemoService's addMemoTx2 Call!");
        Memo memo = new Memo();
        memo.setId(dto.getId());
        memo.setText(dto.getText());
        memoRepository.save(memo);
        throw new SQLException(); // 강제 롤백 유도
    }
}
```

---

### ✅ TxTestServiceTest.java

```java
@SpringBootTest
class TxTestServiceTest {

    @Autowired
    private TxTestService txTestService;

    @Test
    void t2() throws Exception {
        txTestService.addMemoTx(new MemoDto(1, "TEST1"));
    }

    @Test
    void t3() throws Exception {
        txTestService.addMemoTx2(new MemoDto(1, "TEST1"));
    }
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Configuration`: 해당 클래스가 설정 클래스임을 명시
    
- `@EnableTransactionManagement`: 트랜잭션 사용 가능하도록 설정
    
- `@EnableJpaRepositories`: JPA Repository 스캔 및 트랜잭션 매니저 지정
    
- `@Service`: 서비스 레이어를 나타내는 빈 등록 어노테이션
    
- `@Slf4j`: 로그 출력을 위한 롬복 어노테이션
    
- `@Transactional`: 해당 메서드를 트랜잭션으로 묶음
    

---

### ✅ 주요 메서드 설명

#### `addMemoTx()`

- JDBC 기반 트랜잭션
    
- `memoMapper.insert()` 실행 후 `SQLException` 강제 발생 → 롤백 확인용
    

#### `addMemoTx2()`

- JPA 기반 트랜잭션
    
- `memoRepository.save()` 실행 후 `SQLException` 강제 발생 → 롤백 확인용
    

---

### ✅ 트랜잭션 매니저 설정

- `transactionManager`: JDBC 기반 트랜잭션용 (`DataSourceTransactionManager`)
    
- `jpaTransactionManager`: JPA용 트랜잭션 (`JpaTransactionManager`), `@EnableJpaRepositories`에 등록
    

---

### ✅ 테스트 설명

- `t2()`: JDBC 트랜잭션 테스트 → Mapper 사용 후 강제 예외 발생
    
- `t3()`: JPA 트랜잭션 테스트 → Repository 사용 후 강제 예외 발생
    

---

## 📌 요약

- `TxConfig`에서 JDBC용과 JPA용 트랜잭션 매니저를 **동시에 설정**함.
    
- `@Transactional`의 `transactionManager` 속성으로 **트랜잭션 종류를 구분하여 명시적 설정** 가능.
    
- 테스트 클래스에서 각각의 트랜잭션 처리 방식에 대해 **롤백 동작을 확인**할 수 있음.
    
- 실무에서는 데이터 접근 방식(JDBC vs JPA)에 따라 트랜잭션 매니저를 분리 설정하는 패턴이 유용함.
    

필요 시, 각 트랜잭션 동작 결과(DB 반영 여부)를 로그나 DB에서 확인 가능.