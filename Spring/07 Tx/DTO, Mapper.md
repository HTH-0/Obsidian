
---

# 📘 MemoMapper.java & MemoDto.java

## 📦 MemoMapper.java 전체 코드

```java
package com.example.app.domain.mapper;

import java.util.List;

import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Select;

import com.example.app.domain.dto.MemoDto;

public interface MemoMapper {

    @Insert("insert into tbl_memo(id, text, writer, createAt) values(#{id}, #{text}, #{writer}, #{createAt})")
    public int insert(MemoDto dto);

    @Select("select * from tbl_memo order by id desc")
    public List<MemoDto> selectAll();
}
```

---

## 📦 MemoDto.java 전체 코드

```java
package com.example.app.domain.dto;

import java.time.LocalDateTime;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class MemoDto {
    private int id;
    private String text;
    private String writer;
    private LocalDateTime createAt;
}
```

---

## 🔍 코드 분석

### ✅ MemoMapper 설명

- `@Insert`  
    → MyBatis 어노테이션을 이용한 SQL 직접 정의  
    → `MemoDto`의 필드를 그대로 `tbl_memo` 테이블에 매핑하여 insert 실행
    
- `@Select`  
    → 전체 메모를 id 역순으로 조회 (`selectAll()` 메서드에 사용됨)
    

→ **SQL XML Mapper를 따로 사용하지 않고, 어노테이션 기반으로 구현되어 간단 명료함**

### ✅ MemoDto 설명

- `@Data`  
    → Lombok 어노테이션. Getter, Setter, toString, equals, hashCode 자동 생성
    
- `@AllArgsConstructor`, `@NoArgsConstructor`  
    → 전체 필드 생성자 / 기본 생성자 자동 생성
    
- `@Builder`  
    → 빌더 패턴을 사용한 객체 생성 지원 (`MemoDto.builder()...build()` 형태로 사용 가능)
    
- 주요 필드:
    
    - `id`: PK
        
    - `text`: 메모 본문
        
    - `writer`: 작성자
        
    - `createAt`: 작성 시각
        

---

## 📌 요약

- `MemoMapper`는 어노테이션 기반 MyBatis 매퍼로, 메모 등록과 전체 조회를 수행
    
- `MemoDto`는 메모 데이터를 표현하는 자바 객체로, 트랜잭션 대상 데이터 구조의 기준이 됨
    
- `MemoServiceImpl`에서는 이 두 컴포넌트를 통해 DB에 트랜잭션 기반 insert 작업을 수행
    

---
