# ⚙️ MybatisConfig - MyBatis 설정 클래스

## 📦 전체 코드

```java
package com.example.app.config;

import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;

@Configuration
public class MybatisConfig {

	@Autowired
	private DataSource dataSource3;

	@Bean
	public SqlSessionFactory sqlSessionFactory() throws Exception {
		SqlSessionFactoryBean sessionFactory = new SqlSessionFactoryBean();
		sessionFactory.setDataSource(dataSource3);
		
		// Mapper XML 파일의 위치 설정
		PathMatchingResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
		Resource[] resources = resolver.getResources("classpath*:mapper/*.xml");
		sessionFactory.setMapperLocations(resources);
		
		return sessionFactory.getObject();
	}
	
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Configuration`: 스프링 설정 클래스임을 명시. Bean 등록 등을 위한 Java Config 클래스.
    
- 클래스명 `MybatisConfig`: MyBatis 관련 설정을 담당.
    

---

### ✅ 주요 메서드 설명

#### `sqlSessionFactory()`

- 반환 타입: `SqlSessionFactory`
    
- 역할: MyBatis에서 SQL 실행을 위한 핵심 객체 생성
    
- 주요 설정:
    
    - `setDataSource(dataSource3)`: `HikariDataSource` 혹은 다른 커넥션풀로 설정된 `DataSource`를 연결함
        
    - `setMapperLocations(...)`: XML 매퍼 파일들을 등록 (예: `/src/main/resources/mapper/*.xml`)
        
- `sessionFactory.getObject()`를 통해 실제 `SqlSessionFactory` 인스턴스를 생성하여 반환
    

---

### ✅ 의존성 주입 (DI)

- `@Autowired private DataSource dataSource3`: `root-context.xml` 또는 `@Configuration` 클래스에서 정의된 Bean `dataSource3` 주입
    
    - 주로 HikariCP 또는 commons-dbcp2 등으로 구성됨
        

---

### ✅ 기타 주요 로직 설명

- `PathMatchingResourcePatternResolver` 사용:
    
    - `classpath*:` 접두사는 클래스패스의 모든 위치에서 매칭되는 파일 검색 가능
        
    - `mapper/*.xml`: 매퍼 XML 파일을 자동 스캔해서 등록
        

---

## 📌 요약

- 이 설정 클래스는 MyBatis의 `SqlSessionFactory`를 직접 생성하고 스프링에 Bean으로 등록함
    
- 외부 매퍼 XML들을 자동으로 인식하게 설정 (`classpath*:mapper/*.xml`)
    
- `dataSource3`를 통해 실제 DB 연결과 매핑됨
    
- `@Configuration + @Bean` 조합으로 순수 Java 설정 기반 구성
    

> `MybatisConfig`는 `mybatis-spring`의 핵심 설정 클래스로, Spring과 MyBatis를 연결하는 중심 역할을 함.