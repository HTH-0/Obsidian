# 🧾 MemoMapper.java - MyBatis Mapper 인터페이스 정리

## 📦 전체 코드

```java
package com.example.app.domain.mapper;

import java.util.List;
import java.util.Map;

import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Result;
import org.apache.ibatis.annotations.Results;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.SelectKey;
import org.apache.ibatis.annotations.Update;

import com.example.app.domain.dto.MemoDto;

@Mapper
public interface MemoMapper {
	@SelectKey(statement="select max(id)+1 from tbl_memo",keyProperty = "id",before = false, resultType = int.class)
	@Insert(value = "INSERT INTO tbl_memo VALUES (#{id}, #{text}, #{writer}, #{createAt})")
	public int insert(MemoDto memoDto);
	
	@Update("update tbl_memo set text=#{text} where id=#{id}")
	public int update(MemoDto dto);
	
	@Delete("delete from tbl_memo where id=#{id}")
	public int delete(int id);
	
	@Select("select * from tbl_memo where id=#{id}")
	public MemoDto selectAt(int id);
	
	@Select("select * from tbl_memo")
	public List<MemoDto> selectAll(); 

	@Results(id="MemoResultMap", value= {
		@Result(property = "id", column="id"),
		@Result(property = "text", column="text")
	})
	@Select("select * from tbl_memo")
	public List<Map<String,Object>> selectAllResultMap(); 
	
	// xml 방식
	public int insertXml(MemoDto memoDto);
	public int updateXml(MemoDto memoDto);
	public int deleteXml(@Param("id") int id);
	public MemoDto selectAtXml(int id);
	public List<MemoDto> selectAllXml();
	public List<Map<String, Object>> selectAllResultMapXml();
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Mapper`: MyBatis에서 Mapper 인터페이스로 인식하게 함 (컴포넌트 스캔 대상)
    
- 이 인터페이스는 DB의 `tbl_memo` 테이블과 연동
    

---

### ✅ 어노테이션 기반 SQL 메서드

#### `insert(MemoDto memoDto)`

- `@SelectKey`: insert 전에 `max(id)+1`을 조회하여 id 값을 설정
    
    - `before = false`: insert 후 실행되며, 결과는 DTO의 `id`에 자동 설정
        
- `@Insert`: SQL 직접 작성 방식
    
- 필드: id, text, writer, createAt
    

#### `update(MemoDto dto)`

- 해당 id를 가진 메모의 `text`만 수정
    

#### `delete(int id)`

- 특정 id의 메모 삭제
    

#### `selectAt(int id)`

- 단건 조회 → `MemoDto` 반환
    

#### `selectAll()`

- 전체 조회 → `List<MemoDto>` 반환
    

#### `selectAllResultMap()`

- `@Results`: ResultMap 지정
    
- `List<Map<String, Object>>` 형태로 조회 (DTO 대신 key-value 쌍으로 접근할 때 사용)
    

---

### ✅ XML 기반 SQL 메서드

```java
public int insertXml(MemoDto memoDto);
public int updateXml(MemoDto memoDto);
public int deleteXml(@Param("id") int id);
public MemoDto selectAtXml(int id);
public List<MemoDto> selectAllXml();
public List<Map<String, Object>> selectAllResultMapXml();
```

- 위 메서드들은 XML 매퍼 파일에서 구현 내용을 분리하여 관리
    
- 각 메서드는 `<mapper>` XML의 `<insert>`, `<update>`, `<select>`와 매핑됨
    
- `@Param`은 파라미터가 2개 이상이거나 명시적으로 매핑할 필요가 있을 때 사용
    

---

## 📌 요약

- 이 인터페이스는 `tbl_memo` 테이블에 대한 CRUD를 정의함
    
- 어노테이션 방식과 XML 방식이 함께 존재함
    
- `@SelectKey`를 사용해 커스텀 방식으로 id 생성
    
- `ResultMap`을 이용해 `Map<String, Object>` 형태로도 조회 가능
    
- 실무에서는 단순 쿼리는 어노테이션, 복잡한 쿼리는 XML로 분리하는 경우가 많음
    
