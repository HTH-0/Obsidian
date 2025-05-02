
---

# ⚙️ DataSourceConfig.java & TxTests.java

## 📦 DataSourceConfig.java 전체 코드

```java
package com.example.app.config;

import javax.sql.DataSource;

import org.apache.commons.dbcp2.BasicDataSource;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DataSourceConfig {

    // commons-dbcp2 기반 DataSource Bean 등록
    @Bean
    public DataSource dataSource3() {
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/testdb");
        dataSource.setUsername("root");
        dataSource.setPassword("1234");

        dataSource.setInitialSize(5);
        dataSource.setMaxTotal(10);
        dataSource.setMaxIdle(8);
        dataSource.setMinIdle(3);

        return dataSource;
    }
}
```

---

## 🔍 DataSourceConfig 설명

- `@Configuration`  
    → 이 클래스는 Spring Java 설정 클래스임을 명시
    
- `@Bean public DataSource dataSource3()`  
    → commons-dbcp2의 `BasicDataSource` 사용하여 DB 연결 풀 구성  
    → 이 Bean은 `TxConfig.java`의 `DataSourceTransactionManager`에 주입됨  
    → URL, 계정, 풀 크기 등을 설정하여 연결 효율성 확보
    

---

## 📦 TxTests.java 전체 코드

```java
package TxTests;

import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import com.example.app.domain.dto.MemoDto;
import com.example.app.domain.service.MemoServiceImpl;

import java.time.LocalDateTime;

@ExtendWith(SpringExtension.class)
@ContextConfiguration(locations = {
    "file:src/main/webapp/WEB-INF/spring/root-context.xml"
})
public class TxTests {

    @Autowired
    private MemoServiceImpl memoService;

    @Test
    public void 트랜잭션테스트() {
        MemoDto dto = MemoDto.builder()
                .id(1)
                .text("트랜잭션 테스트")
                .writer("관리자")
                .createAt(LocalDateTime.now())
                .build();

        memoService.addMemoTx(dto);
    }
}
```

---

## 🔍 TxTests 설명

- `@ExtendWith(SpringExtension.class)`  
    → Spring JUnit5 테스트 환경과 통합
    
- `@ContextConfiguration(...)`  
    → 테스트에서 사용할 Spring 설정 XML 지정 (`root-context.xml`에서 모든 Java 설정을 import)
    
- `memoService.addMemoTx(dto)`  
    → 예외 발생을 유도하여 트랜잭션이 실제로 **rollback** 되는지 테스트
    

→ `MemoServiceImpl`의 `addMemoTx()`는 두 번 insert를 시도하고, 두 번째에서 PK 충돌 발생 → 전체 롤백 확인

---

## 📌 요약

- `DataSourceConfig`는 트랜잭션 매니저의 기반이 되는 DB 연결 풀 설정을 담당
    
- `TxTests`는 실제 `@Transactional`이 선언된 메서드가 예외 상황에서 rollback 되는지 확인하는 **단위 테스트 클래스**
    
- 설정-서비스-테스트까지 트랜잭션 처리 흐름을 일관되게 구성한 예제
    

---

다음으로 정리할 내용은 `TxConfig`, `MemoServiceImpl`, `Mapper`, `DTO`, `Config`, `Test` 까지 모두 마무리되었고,  
이제 전체 흐름을 한눈에 정리하는 **최종 요약 Step-by-Step 흐름도**를 작성해줄까?