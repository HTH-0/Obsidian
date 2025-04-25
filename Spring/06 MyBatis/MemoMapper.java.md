# 🧭 `MemoMapper.java` 분석 - MyBatis Mapper 인터페이스

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
			@Result(property = "id",column="id"),
			@Result(property = "text", column="text")
	})
	@Select("select * from tbl_memo")
	public List< Map<String,Object> > selectAllResultMap(); 
	
	// xml 방식
	public int insertXml(MemoDto memoDto);
	public int updateXml(MemoDto memoDto);
	public int deleteXml(@Param("id") int id);
	public MemoDto selectAtXml(int id);
	public List<MemoDto> selectAllXml();
	public List<Map<String, Object>> selectAllResultMapXml();
	public List<Map<String,Object>> Select_if_xml(Map<String,Object> param);
	public List<Map<String, Object>> Select_when_xml(Map<String, Object> param);
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Mapper`  
    → MyBatis가 이 인터페이스를 Mapper로 인식하게 해줌 (Spring이 자동으로 구현체 생성)
    

### ✅ 어노테이션 기반 SQL 매핑 메서드

- `@SelectKey` + `@Insert`
    
    ```java
    @SelectKey(statement="select max(id)+1 from tbl_memo", keyProperty = "id", before = false, resultType = int.class)
    @Insert("INSERT INTO tbl_memo VALUES (#{id}, #{text}, #{writer}, #{createAt})")
    ```
    
    → `id`를 DB에서 미리 생성하지 않고, insert 이후에 `max(id)+1`로 수동 생성  
    → `before = false`로 인해 insert 실행 후 selectKey 실행됨
    
- `@Update`, `@Delete`, `@Select`  
    → 간단한 쿼리일 경우 어노테이션만으로 처리 가능  
    → `#{}`를 통해 파라미터 바인딩
    
- `@Results`, `@Result`  
    → `ResultMap` 지정, 반환 타입이 DTO가 아니라 `Map<String,Object>`일 때 명시적 매핑 필요
    

### ✅ XML 기반 매핑 메서드

- 다음 메서드들은 `MemoMapper.xml`에 매핑된 SQL 구문을 따로 정의해야 함
    
    ```java
    public int insertXml(MemoDto memoDto);
    public int updateXml(MemoDto memoDto);
    public int deleteXml(@Param("id") int id);
    public MemoDto selectAtXml(int id);
    public List<MemoDto> selectAllXml();
    public List<Map<String, Object>> selectAllResultMapXml();
    public List<Map<String,Object>> Select_if_xml(Map<String,Object> param);
    public List<Map<String, Object>> Select_when_xml(Map<String, Object> param);
    ```
    
- 특히 `Select_if_xml`, `Select_when_xml` 은 `<if>`, `<choose>` 와 같은 동적 SQL 태그를 이용해 조건 처리 수행
    

---

## 📌 요약

- `MemoMapper`는 `MemoDto`와 연결된 MyBatis 매퍼 인터페이스
    
- 간단한 SQL은 어노테이션 방식으로 직접 정의 (`@Insert`, `@Select`, `@Update`, `@Delete`)
    
- 복잡한 SQL은 XML 파일에서 매핑 (동적 조건문 포함)
    
- `@SelectKey`는 PK를 수동으로 생성할 때 사용되며, DB의 auto_increment를 쓰지 않는 방식
    
- `@Results`는 반환값이 Map일 때 필요한 컬럼-필드 매핑 설정
    
