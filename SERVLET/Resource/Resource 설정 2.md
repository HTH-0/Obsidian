# ⚙️ JDBC 커넥션 풀 설정 요약 (Mysql DataSource 기반)

---

## 📄 context.xml 설정 요약 (Tomcat)

```xml
<Context>
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- CONNECTION POOL(DataSource) -->
    <Resource 
        driverClassName="com.mysql.cj.jdbc.Driver"
        url="jdbc:mysql://localhost/testDb"
        username="root"
        password="1234"
        name="jdbc/MysqlDB"
        type="javax.sql.DataSource"
        auth="Container"
        maxActive="10"
        maxIdle="2"
        maxWait="5000" />
</Context>
```

---

## 🧠 설정 설명

|속성|의미|
|---|---|
|`name="jdbc/MysqlDB"`|리소스 이름, JNDI에서 참조할 이름. Java 코드에서 lookup할 때 사용됨|
|`driverClassName`|사용하는 DB 드라이버. MySQL 8.x 기준 `com.mysql.cj.jdbc.Driver`|
|`url`|DB 접속 URL (ex: `jdbc:mysql://localhost/testDb`)|
|`username` / `password`|DB 접속 계정|
|`type="javax.sql.DataSource"`|JDBC에서 사용하는 인터페이스로 커넥션 풀 관리|
|`auth="Container"`|WAS가 인증 책임을 가짐 (JNDI 표준 설정)|
|`maxActive`|동시에 사용할 수 있는 최대 연결 수|
|`maxIdle`|유휴 상태로 유지할 최대 연결 수|
|`maxWait`|커넥션 부족 시 대기 최대 시간 (ms 단위)|

---

## 🧩 연동 흐름 요약

```
[Tomcat context.xml]
  └── Resource 설정(name="jdbc/MysqlDB")

→ [web.xml]
  └── <resource-ref>에서 이 리소스를 참조

→ [MysqlDbUtils.java]
  └── InitialContext → lookup("java:/comp/env/jdbc/MysqlDB")
       → DB 연결 획득 및 쿼리 처리
```

---

## ✅ 체크리스트

- `context.xml`에 `name="jdbc/MysqlDB"` 설정과  
    `MysqlDbUtils`의 lookup 경로가 정확히 일치해야 함
    
- 드라이버 버전에 따라 `com.mysql.jdbc.Driver`가 아니라  
    `com.mysql.cj.jdbc.Driver`를 사용해야 함 (현재는 ✅ 적용됨)
    
- `testDb`, 계정 정보는 실제 DB 환경에 맞게 수정 필요
    

---
