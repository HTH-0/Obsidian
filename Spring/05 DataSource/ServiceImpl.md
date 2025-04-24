# 📝 메모 등록 서비스 구현 클래스

## 📦 전체 코드

```java
package com.example.app.domain.Service;

import java.sql.SQLException;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.example.app.domain.Dao.MemoDaoImpl;
import com.example.app.domain.Dto.MemoDto;

@Service
public class MemoServiceImpl {
	
	@Autowired
	private MemoDaoImpl memoDaoImpl;
	
	public boolean registrationMemo(MemoDto memoDto) throws SQLException {
		int result = memoDaoImpl.insert(memoDto);
		return result > 0;
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Service`:
    
    - 해당 클래스가 서비스 계층(비즈니스 로직 담당)임을 스프링에 알림.
        
    - 스프링이 이 클래스를 빈으로 등록하고, 의존성 주입에 사용할 수 있게 함.
        
- `MemoServiceImpl`:
    
    - 메모 등록 기능을 수행하는 서비스 구현 클래스.
        

### ✅ 주요 메서드 설명

- `registrationMemo(MemoDto memoDto)`:
    
    - 메모 정보를 받아 DB에 저장 요청.
        
    - 내부적으로 `MemoDaoImpl`의 `insert()` 메서드를 호출하여 저장 처리.
        
    - 삽입 결과가 0보다 크면 `true`, 아니면 `false` 반환.
        

### ✅ 의존성 주입 (DI)

- `@Autowired`:
    
    - `MemoDaoImpl`을 자동 주입받아 사용.
        
    - 외부에서 명시적으로 객체를 생성하지 않고도 DAO 기능 사용 가능.
        

### ✅ 기타 주요 로직 설명

- `throws SQLException`:
    
    - DAO 레이어에서 발생할 수 있는 예외를 서비스 계층에서 그대로 위임.
        
    - 나중에 컨트롤러에서 예외 처리를 글로벌 핸들러 등으로 처리할 수 있음.
        

---

## 📌 요약

- 이 코드는 메모 등록 비즈니스 로직을 담당하는 `Service` 계층 클래스.
    
- `MemoDto`를 받아 DAO로 전달하여 DB에 insert 실행.
    
- 성공 여부는 boolean 값으로 반환.
    
- `@Service`, `@Autowired`로 구성된 전형적인 스프링 DI + 서비스 구조 예제.