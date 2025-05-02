
---

# 🧾 MemoServiceImpl.java: 트랜잭션 처리 예제

## 📦 전체 코드

```java
package com.example.app.domain.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import com.example.app.domain.dto.MemoDto;
import com.example.app.domain.mapper.MemoMapper;

import lombok.extern.slf4j.Slf4j;

@Service
@Slf4j
public class MemoServiceImpl {

	@Autowired
	private MemoMapper mapper;

	// 전체 메모 조회
	public List<MemoDto> getAllMemo() {
		log.info("MemoServiceImpl's getAllMemo Call!");
		return mapper.selectAll();
	}

	// 메모 1건 등록 (트랜잭션 없음)
	public void addMemo(MemoDto dto) {
		log.info("MemoServiceImpl's addMemo Call!");
		mapper.insert(dto);
	}

	// 메모 2건 등록 (트랜잭션 적용)
	@Transactional(rollbackFor = Exception.class)
	public void addMemoTx(MemoDto dto) {
		log.info("MemoServiceImpl's addMemoTx Call!");
		int id = dto.getId();

		mapper.insert(dto);   // 첫 번째 정상 INSERT
		dto.setId(id);        // 동일한 PK로 설정하여 중복 오류 유도
		mapper.insert(dto);   // 두 번째 INSERT → PK 중복 오류 발생 예정
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Service`  
    → 비즈니스 로직을 수행하는 서비스 클래스임을 명시
    
- `@Slf4j`  
    → 로깅을 위한 Lombok 어노테이션 (`log.info(...)` 가능하게 함)
    

### ✅ 주요 메서드 설명

- `getAllMemo()`  
    → 전체 메모 리스트 조회. DB에서 모든 메모를 불러오는 단순 쿼리
    
- `addMemo()`  
    → 메모 1건을 단순 등록. 트랜잭션을 명시적으로 적용하지 않음
    
- `addMemoTx()`  
    → 트랜잭션을 테스트하기 위한 메서드  
    → `@Transactional(rollbackFor = Exception.class)`으로 예외 발생 시 전체 롤백  
    → 두 번째 `insert()`에서 고의적으로 PK 중복 예외를 발생시켜 롤백이 일어나는지 확인
    

### ✅ 트랜잭션 적용 설명

- `@Transactional(rollbackFor = Exception.class)`  
    → 이 메서드 전체가 하나의 트랜잭션으로 처리됨  
    → 하나라도 예외 발생 시 DB 변경사항 전부 롤백  
    → `unchecked exception`이 아닌 일반 `Exception`까지 롤백 대상으로 설정함
    

---

## 📌 요약

- `MemoServiceImpl`은 트랜잭션 처리 실습의 핵심 서비스로, 정상 케이스와 실패 케이스를 비교할 수 있게 구성됨
    
- `addMemoTx()` 메서드는 고의적으로 예외 상황을 발생시켜 트랜잭션이 롤백되는 것을 확인할 수 있도록 설계됨
    
- Spring에서는 `@Transactional`과 `TxConfig` 설정을 통해 선언적으로 트랜잭션을 매우 간단하게 처리할 수 있음
    

---
