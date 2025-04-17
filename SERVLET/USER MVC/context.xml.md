# ⚙️ 10번 프로젝트: context.xml 설정 정리

---

## 📄 context.xml 전체 코드

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- CONNECTION POOL(XADataSource) -->
    <Resource name="jdbc/MysqlDB"
              auth="Container"
              type="javax.sql.XADataSource"
              factory="Domain.Dao.ConnectionPool.MysqlXADataSourceFactory"
              url="jdbc:mysql://localhost:3306/testDB"
              user="root"
              password="1234"/>
</Context>
```

---

## 🧠 설명

|항목|의미|
|---|---|
|`Resource`|JNDI로 등록할 DB 커넥션 리소스 정의|
|`name`|애플리케이션에서 참조할 JNDI 이름 (`jdbc/MysqlDB`)|
|`type`|리소스 타입 (여기선 `javax.sql.XADataSource`, 즉 분산 트랜잭션용)|
|`factory`|JNDI 요청 시 리소스 객체를 생성할 팩토리 클래스 지정|
|`url`, `user`, `password`|DB 접속 정보|

---

## ✅ 특징

- 단순 커넥션 풀이 아닌 **XA 기반 커넥션 풀 설정**
    
- `MysqlXADataSourceFactory`를 통해 `MysqlXADataSource` 객체를 생성하고, 분산 트랜잭션 처리 가능
    

---

## 🔁 전체 흐름에서의 역할

1. DAO 또는 ConnectionPool 내부에서 `InitialContext.lookup("java:/comp/env/jdbc/MysqlDB")` 호출
    
2. 위 설정을 통해 커넥션 풀을 찾고 커넥션을 획득
    
3. 트랜잭션 컨트롤: `ConnectionPool.beginTransaction() → commit() or rollback()`
    

---


