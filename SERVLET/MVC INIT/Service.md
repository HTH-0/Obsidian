# 🧩 UserServiceImpl 클래스 정리

---

## 📄 UserServiceImpl.java

```java
package Domain.Service;

import java.sql.SQLException;

import Domain.Dao.UserDao;
import Domain.Dao.UserDaoImpl;
import Domain.Dao.ConnectionPool.ConnectionPool;
import Domain.Dto.UserDto;

public class UserServiceImpl {

	private UserDao userDao;
	private ConnectionPool connectionPool;

	// 싱글톤
	private static UserServiceImpl instance;
	private UserServiceImpl() throws Exception {
		userDao = UserDaoImpl.getInstance();
		connectionPool = ConnectionPool.getInstance();
		System.out.println("[SERVICE] UserServiceImpl init...");
	}
	public static UserServiceImpl getInstance() throws Exception {
		if (instance == null)
			instance = new UserServiceImpl();
		return instance;
	}

	// 회원가입 기능 (트랜잭션 포함)
	public boolean userJoin(UserDto userDto) throws Exception {
		boolean isJoin = false;
		try {
			connectionPool.beginTransaction();

			isJoin = userDao.insert(userDto) > 0;

			connectionPool.commitTransaction();
		} catch (SQLException e) {
			connectionPool.rollbackTransaction();
		}
		return isJoin;
	}

	// 회원조회
	// 회원정보수정
	// 회원탈퇴
	// 로그인
	// 로그아웃
}
```

---

## 🧠 설명

### ✅ 구조 및 구성

- `userDao`: DB 접근을 위한 DAO 객체
    
- `connectionPool`: 직접 구현한 커넥션 풀 (분산 트랜잭션 기능 포함)
    

### ✅ 싱글톤 패턴

- 외부에서 `getInstance()`로 접근 → 객체 단일화
    

---

## ✅ 핵심 기능: `userJoin()`

|단계|처리|
|---|---|
|1|`connectionPool.beginTransaction()`으로 트랜잭션 시작|
|2|`userDao.insert(userDto)`로 사용자 DB 저장|
|3|성공 시 `commitTransaction()` 호출|
|4|실패 시 `rollbackTransaction()` 호출|
|5|최종 성공 여부를 boolean으로 반환|

### 📌 트랜잭션 활용 포인트

- 다중 쿼리 처리(주석 처리된 insert 3줄 참고) 시 중간 실패가 발생하면 전체 롤백되어야 하므로 **트랜잭션이 필수**
    
- 현재는 단일 insert만 있으나 구조는 확장 가능
    

---

## 🔁 연계 흐름 요약

```
UserCreateController
   ↓
UserServiceImpl.userJoin()
   ↓
ConnectionPool.beginTransaction()
   ↓
UserDaoImpl.insert()
   ↓
ConnectionPool.commit or rollback
```

---
