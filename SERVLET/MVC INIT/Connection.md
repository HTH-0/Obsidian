# 🔄 커스텀 커넥션 풀 및 분산 트랜잭션 구조 정리 (09 프로젝트)

---

## 📦 구조 요약

|클래스명|역할 요약|
|---|---|
|`ConnectionItem`|커넥션 1개 + XA 트랜잭션 관련 정보 보관 객체|
|`ConnectionPool`|커넥션 풀 관리 + 분산 트랜잭션 처리 (2PC)|
|`MysqlXADataSourceFactory`|JNDI 리소스를 통해 `MysqlXADataSource`를 생성하는 팩토리|

---

## 📄 ConnectionItem.java

```java
public class ConnectionItem {
	private Connection conn;
	private XAResource xaResource;
	private Xid xid;
	private boolean isUse;
	private boolean inTransaction;

	// 생성자
	public ConnectionItem(Connection conn, XAResource xaResource) {
		this.conn = conn;
		this.xaResource = xaResource;
		this.isUse = true;
	}
	// Getter/Setter 정의됨
}
```

### ✅ 주요 특징

- 단일 DB 커넥션과 그에 대한 XAResource, Xid, 상태값 관리
    
- 분산 트랜잭션 중 사용 여부(`isUse`), 트랜잭션 참여 여부(`inTransaction`) 플래그 보유
    

---

## 📄 ConnectionPool.java

```java
public class ConnectionPool {
	private List<ConnectionItem> connectionPool;
	private final int size = 10;

	private static ConnectionPool instance;
	public static ConnectionPool getInstance() throws SQLException

	public synchronized ConnectionItem getConnection()
	public synchronized void releaseConnection(ConnectionItem connItem)
	
	// 분산 트랜잭션 처리 메서드
	public void beginTransaction()
	public void commitTransaction()
	public void rollbackTransaction()
```

### ✅ 일반 커넥션 관리

- 커넥션 10개를 미리 만들어 `ConnectionItem`으로 관리
    
- `getConnection()` → 사용 가능한 커넥션 제공
    
- `releaseConnection()` → 커넥션 반납 처리
    

### ✅ 분산 트랜잭션 (XA 기반)

- **`beginTransaction()`**
    
    - `Xid` 생성 후 각 커넥션에 `start()` 호출
        
- **`commitTransaction()`**
    
    - 2PC(2단계 커밋) 방식
        
        1. **end()**
            
        2. **prepare()**
            
        3. **commit()**
            
- **`rollbackTransaction()`**
    
    - 실패 시 모든 XA 커넥션에 `rollback()` 호출
        

---

## 📄 MysqlXADataSourceFactory.java

```java
public class MysqlXADataSourceFactory implements ObjectFactory {
	@Override
	public Object getObjectInstance(Object obj, ...) throws Exception {
		Reference ref = (Reference) obj;
		MysqlXADataSource ds = new MysqlXADataSource();
		ds.setUrl(...);
		ds.setUser(...);
		ds.setPassword(...);
		return ds;
	}
}
```

### ✅ 설명

- JNDI 환경에서 `MysqlXADataSource` 객체를 생성하기 위한 팩토리 클래스
    
- `context.xml`의 `<Resource>` 설정과 연동 가능
    

---

## 🧠 전체 요약

- 일반 JDBC에서 사용하던 `Connection` + `DriverManager` 방식 → ❌
    
- **커넥션 풀 & XA 트랜잭션 기반 구조로 완전히 커스터마이징됨** ✅
    
- DAO 클래스들은 `ConnectionPool.getConnection()`으로 커넥션을 얻고,  
    필요 시 분산 트랜잭션 시작/커밋/롤백을 처리 가능
    
- 향후 여러 DAO 작업이 하나의 트랜잭션으로 묶여야 할 때 `begin → commit/rollback` 흐름 적용 가능
    

---
