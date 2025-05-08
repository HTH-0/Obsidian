# ✅ Spring Boot에서 DataSource 설정 - JavaConfig

## 📦 전체 코드

```java
package com.example.demo.config;

import com.zaxxer.hikari.HikariDataSource;
import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class DataSourceConfig {
    
    // Spring-JDBC + Apache DBCP2 사용
    @Bean
    public DataSource dataSource2() {
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/bookdb");
        dataSource.setUsername("root");
        dataSource.setPassword("1234");

        dataSource.setInitialSize(5);   // 초기 커넥션 수
        dataSource.setMaxTotal(10);     // 최대 커넥션 수
        dataSource.setMaxIdle(8);       // 최대 유휴 커넥션 수
        dataSource.setMinIdle(3);       // 최소 유휴 커넥션 수

        return dataSource;
    }

    // HikariCP 설정
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

- `@Configuration`: 이 클래스가 Spring 설정 클래스임을 나타냄. Java 기반 Bean 설정 파일로 사용됨.
    
- `@Bean`: Spring 컨테이너에 직접 Bean으로 등록할 객체 정의.
    

---

### ✅ 주요 메서드 설명

#### `dataSource2()`

- Apache Commons DBCP2를 사용하는 커넥션 풀 설정
    
- 커넥션 초기 개수, 최대/최소 유휴 커넥션, 총 커넥션 수 등을 세부적으로 조정 가능
    
- `BasicDataSource`는 성능 면에서는 HikariCP보다 느릴 수 있으나, 설정이 직관적
    

#### `dataSource3()`

- HikariCP 기반의 커넥션 풀 설정
    
- Spring Boot 기본 DataSource (별도 설정 없으면 Hikari 사용됨)
    
- `JdbcUrl`, `username`, `password`를 설정해 커넥션 풀 구성
    

---

### ✅ 기타 주요 설정 설명

- `bookdb`: 실제 사용되는 데이터베이스명. `application.properties`와 연결됨
    
- `com.mysql.cj.jdbc.Driver`: MySQL 8.x 드라이버 클래스명
    

---

## 📌 요약

- `DataSourceConfig` 클래스는 `@Configuration` 기반의 수동 DataSource 설정 예제
    
- `dataSource2()`는 Apache Commons DBCP2, `dataSource3()`는 HikariCP를 이용한 커넥션 풀 정의
    
- `application.properties`에서 자동 설정을 끄고 이 설정을 직접 등록해 사용 가능
    
- Spring Boot에서는 HikariCP가 기본 설정이므로, 커스텀 커넥션 풀을 사용하고 싶을 경우 이와 같은 방식으로 명시적으로 등록해야 함
    

---
