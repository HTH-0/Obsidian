# 🗂️ Root Context XML 설정 (root-context.xml)

## 📦 전체 코드

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
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

	<!-- DataSource Bean 등록 -->
	<bean id="dataSource1" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://localhost:3306/testDB" />
		<property name="username" value="root" />
		<property name="password" value="1234" />
	</bean>
		
	<!-- 자동 빈 등록 설정 -->
	<context:component-scan base-package="com.example.app.config" />
	<context:component-scan base-package="com.example.app.domain.Dao" />
	<context:component-scan base-package="com.example.app.domain.Service" />

	<!-- 
		에플리케이션 전체에서 공유되는 설정 파일
		전역 설정 or Bean의 정의
		주로 비즈니스 로직과 관련된 서비스, Dao 등을 설정
		데이터베이스 연결, 트랜잭션 관리, 보안 설정(Spring Security 등..) 과 같은 애플리케이션 전체에서 공유되는 설정을 포함
		딱 한번 생성되는 Bean들이 정의되며, 모든 웹 요청에 공유
	 -->	
</beans>
```

---

## 🔍 코드 분석

### ✅ 직접 등록한 Bean

- `personDto1`, `personDto2`:
    
    - `PersonDto` 객체를 XML에서 직접 등록
        
    - 각각 다른 값을 가진 생성자 인자를 통해 객체 초기화
        

### ✅ DataSource Bean 등록

- `dataSource1`:
    
    - DB 연결을 위한 `DriverManagerDataSource` 객체 설정
        
    - JDBC URL, 사용자명, 비밀번호 등을 명시
        
    - DAO 클래스에서 `@Autowired`로 주입받아 사용
        

### ✅ component-scan 설정

- `com.example.app.config`, `com.example.app.domain.Dao`, `com.example.app.domain.Service` 패키지에 존재하는 클래스 중
    
    - `@Component`, `@Repository`, `@Service`, `@Controller` 등을 가진 클래스들을 자동으로 Bean으로 등록
        

### ✅ Root Context 역할

- 전역적으로 공유되는 Bean 설정
    
- Servlet-context와 분리되어 DB, Service, DAO 등 공통 영역 담당
    

---

## 📌 요약

- 이 XML 파일은 스프링 루트 설정 파일로, 전체 프로젝트에서 공유되는 Bean을 정의함.
    
- `@ComponentScan`으로 필요한 패키지를 스캔하고, 직접 등록한 DTO, DataSource 설정 포함.
    
- 전형적인 Spring MVC 구조에서 DAO/Service 계층과 DB 설정을 담당하는 핵심 설정 파일.