
---

# 🌿 Spring TxConfig.java 설정

## 📦 전체 코드

```java
@Configuration
@EnableTransactionManagement
public class TxConfig {
	
	@Autowired
	private DataSource dataSource3;

	@Bean
	public DataSourceTransactionManager transactionManager() {
		return new DataSourceTransactionManager(dataSource3);
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Configuration`  
    → 해당 클래스가 Spring 설정 파일임을 명시 (Java 기반 설정)
    
- `@EnableTransactionManagement`  
    → 트랜잭션 관리 기능을 활성화  
    → `@Transactional`이 붙은 메서드에 대해 트랜잭션 AOP가 적용됨  
    → 내부적으로 **프록시(Proxy) 기반 AOP 방식**을 사용하여 트랜잭션을 자동으로 처리함
    

### ✅ 의존성 주입 (DI)

- `private DataSource dataSource3`  
    → `DataSourceConfig.java`에서 등록한 3번째 DataSource 빈을 주입 받음  
    → 트랜잭션을 적용할 DB 연결 객체
    

### ✅ 트랜잭션 매니저 등록

- `DataSourceTransactionManager`를 수동으로 빈으로 등록
    
- 이 빈을 통해 Spring이 트랜잭션을 관리함
    
- `JdbcTemplate` 또는 MyBatis 등과 함께 사용할 수 있음
    

---

## 📌 요약

- 이 설정 클래스는 Spring의 **선언적 트랜잭션 처리**를 가능하게 해주는 핵심 구성 요소임
    
- `@EnableTransactionManagement`를 통해 `@Transactional` 기반 트랜잭션 처리가 활성화되고,
    
- `DataSourceTransactionManager`는 특정 DataSource(DB 연결)에 대한 트랜잭션을 실제로 수행하는 역할을 함
    

---
