# 🧩 `root-context.xml`의 MyBatis 설정 정리

## 📦 전체 코드

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
	xsi:schemaLocation="http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring-1.2.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">
	
	<!-- Root Context: defines shared resources visible to all other web components -->
		
	<!-- 직접 Bean 등록 -->
	<bean id="personDto1" class="com.example.app.domain.Dto.PersonDto">
		<constructor-arg name="username" value="홍길동" />
		<constructor-arg name="age" value="11" />
		<constructor-arg name="addr" value="창원" />
	</bean>
		
	<bean id="personDto2" class="com.example.app.domain.Dto.PersonDto">
		<constructor-arg name="username" value="홍길동2" />
		<constructor-arg name="age" value="22" />
		<constructor-arg name="addr" value="울산" />
	</bean>

	<bean id="dataSource1" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/testDB" />
		<property name="username" value="root" />
		<property name="password" value="1234" />
	</bean>
		
	<context:component-scan base-package="com.example.app.config" />
	<context:component-scan base-package="com.example.app.domain.Dao" />
	<context:component-scan base-package="com.example.app.domain.Service" />
	
	<mybatis-spring:scan base-package="com.example.app.domain.mapper"/>

</beans>
```

---

## 🔍 코드 분석 (MyBatis 중심)

### ✅ `mybatis-spring` 네임스페이스 설정

```xml
xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
```

- MyBatis를 Spring과 연동하기 위한 네임스페이스 선언
    
- 관련된 태그(`mybatis-spring:scan`)를 사용하기 위해 반드시 필요
    

---

### ✅ MyBatis 매퍼 인터페이스 자동 스캔

```xml
<mybatis-spring:scan base-package="com.example.app.domain.mapper"/>
```

- `@Mapper` 또는 XML 매퍼 없이도, MyBatis 매퍼 인터페이스를 자동으로 빈으로 등록
    
- `base-package` 경로 하위의 모든 매퍼 인터페이스를 감지함
    
- 즉, `com.example.app.domain.mapper` 패키지에 `UserMapper`, `MemoMapper` 등의 인터페이스가 있으면 자동 등록됨
    

---

### ✅ DataSource 설정

```xml
<bean id="dataSource1" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
```

- MyBatis가 사용할 DB 연결 정보 정의
    
- 이후 Java Config나 SqlSessionFactoryBean에서 이 `dataSource1`을 참조하여 DB에 접근하게 됨
    
- 기본적인 설정이므로 성능이나 커넥션 관리 측면에서 실무에서는 HikariCP, DBCP2 등을 선호
    

---

## 💡 MyBatis 설정에 필요한 추가 구성 요소 (보충 필요)

현재 `SqlSessionFactoryBean`, `SqlSessionTemplate` 등의 MyBatis 핵심 Bean 설정이 생략되어 있음. 이 설정이 없다면 XML 기반으로 완전한 MyBatis 연동이 되지 않음.

예시 보충:

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource1" />
</bean>

<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
	<constructor-arg index="0" ref="sqlSessionFactory" />
</bean>
```

---

## 📌 요약

- `<mybatis-spring:scan>`을 통해 `mapper` 인터페이스들을 자동 등록함
    
- `dataSource1`을 설정해 DB 연결 정보를 제공함
    
- 현재 `SqlSessionFactoryBean`, `SqlSessionTemplate` 설정이 없기 때문에 MyBatis 연동이 완전하지 않음 → 이 부분은 보완 필요
    
- 전반적으로 루트 컨텍스트에서는 DAO, Service, Config 등의 핵심 비즈니스 로직과 설정을 담당하며, DB 연동 핵심 요소들도 이곳에 정의함
    

