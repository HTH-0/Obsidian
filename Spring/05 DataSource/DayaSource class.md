# 🗃️ DataSource 설정 클래스

## 📦 전체 코드

```java
package com.example.app.config;

import javax.sql.DataSource;

import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.zaxxer.hikari.HikariDataSource;

@Configuration
public class DataSourceConfig {
	// Spring-jdbc DataSource
	
	@Bean
	public DataSource dataSource2() {

		BasicDataSource dataSource = new BasicDataSource();
		dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
		dataSource.setUrl("jdbc:mysql://localhost:3306/bookdb");
		dataSource.setUsername("root");
		dataSource.setPassword("1234");

		dataSource.setInitialSize(5); 	// 초기 연결 개수
		dataSource.setMaxTotal(10);		// 최대 연결 개수
		dataSource.setMaxIdle(8);		// 최대 유휴 연결 수
		dataSource.setMinIdle(3);		// 최소 유휴 연결 수
		
		return dataSource;
	}
	
	// HikariCP DataSource
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

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Configuration`: 이 클래스가 스프링 설정 클래스임을 명시. Bean 객체를 생성하고 스프링 컨테이너에 등록하는 역할.
    
- `@Bean`: 메서드에서 반환하는 객체를 스프링 빈으로 등록. 이 경우 `DataSource`를 직접 생성하여 Bean으로 등록함.
    

---

### ✅ 주요 메서드 설명

#### 📌 `dataSource2()`: commons-dbcp2 기반 DataSource 생성

- `BasicDataSource` 사용 → Apache commons-dbcp2 커넥션 풀 구현체
    
- `setDriverClassName`, `setUrl`, `setUsername`, `setPassword`: DB 접속 정보 설정
    
- 커넥션 풀 설정:
    
    - `setInitialSize(5)`: 초기 커넥션 5개를 생성해 풀에 보관
        
    - `setMaxTotal(10)`: 최대 커넥션 수 10개 제한
        
    - `setMaxIdle(8)`: 유휴 상태로 유지 가능한 최대 커넥션 수
        
    - `setMinIdle(3)`: 최소 유휴 커넥션 수 확보
        

> 주의: commons-dbcp2는 `BasicDataSource` 클래스를 통해 다양한 커넥션 풀 설정을 제공

---

#### 📌 `dataSource3()`: HikariCP 기반 DataSource 생성

- `HikariDataSource` 사용 → Spring Boot 2.x 기본 커넥션 풀
    
- 설정은 간단하지만 고성능
    
    - `setDriverClassName`, `setJdbcUrl`, `setUsername`, `setPassword`만 설정
        
- HikariCP는 내부적으로 풀링 설정을 자동으로 튜닝하여 성능이 우수함
    

---

### ✅ 의존성 주입 (DI)

- 이 클래스에서 생성한 `dataSource2`, `dataSource3`는 다른 컴포넌트에서 `@Autowired`로 주입받아 사용 가능
    
- 예: DAO 클래스에서 DB 연결에 사용
    

---

## 📌 요약

- 이 클래스는 `@Configuration`을 통해 스프링 설정 클래스로 등록됨
    
- `dataSource2()`는 Apache commons-dbcp2 커넥션 풀을 수동 설정하여 Bean 등록
    
- `dataSource3()`는 HikariCP 커넥션 풀을 사용하여 간결하게 구성
    
- 두 메서드는 `@Bean`으로 선언되어 스프링 컨테이너에 관리되는 `DataSource` 객체를 제공함
    

---

필요하면 이 설정을 `root-context.xml`이 아닌 JavaConfig로 대체할 때 활용 가능.