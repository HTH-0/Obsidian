
---

# 🧩 MyBatis & MyBatis-Spring 의존성 설정

## 📦 pom.xml 의존성 추가

```xml
<!-- MyBatis core -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.19</version>
</dependency>

<!-- MyBatis-Spring 연동 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>2.0.7</version>
</dependency>
```

---

## 🔍 설명

### ✅ MyBatis (3.5.19)

- Java에서 SQL을 XML 또는 Annotation 기반으로 관리할 수 있는 **퍼시스턴스 프레임워크**
    
- JDBC를 추상화하면서도 SQL은 명시적으로 작성할 수 있음
    
- 버전 `3.5.19`은 2024년 기준으로 안정적인 최신 버전 중 하나
    

### ✅ MyBatis-Spring (2.0.7)

- Spring과 MyBatis를 자연스럽게 연동해주는 **연동 모듈**
    
- `SqlSessionFactoryBean`, `SqlSessionTemplate`, 그리고 `@MapperScan` 등 스프링에서 사용할 수 있는 유틸리티 지원
    
- Spring IoC 컨테이너에 쉽게 통합 가능
    
- `2.0.7`은 Spring 5.x, MyBatis 3.5.x와 호환성 좋음
    

---

## ⚙️ 설치 방법

1. `pom.xml`에 위 의존성 추가
    
2. Maven > `Update Project...` 실행 (STS 또는 Eclipse에서 프로젝트 우클릭)
    
3. `root-context.xml` 또는 `Java Config`에 MyBatis 관련 Bean 설정 진행
    

---

## 🧪 설치 확인

- `mybatis` 관련 클래스 인식 여부 확인
    
    ```java
    import org.apache.ibatis.annotations.Mapper;
    import org.apache.ibatis.session.SqlSessionFactory;
    ```
    
- `mybatis-spring` 관련 클래스 인식 여부 확인
    
    ```java
    import org.mybatis.spring.SqlSessionTemplate;
    import org.mybatis.spring.annotation.MapperScan;
    ```
    

---

## 📌 요약

- `mybatis`: 핵심 SQL 프레임워크
    
- `mybatis-spring`: Spring과의 연동 지원
    
- 버전 호환성: `3.5.19` + `2.0.7`은 Spring 5.x 환경에서 적절함
    
- 의존성 추가 후 반드시 Maven 프로젝트 업데이트 필요
    

---
