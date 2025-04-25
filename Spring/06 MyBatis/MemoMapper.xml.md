# 🧾 MemoMapper.xml - MyBatis 매퍼 파일 분석

## 📦 전체 코드

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.app.domain.mapper.MemoMapper">

    <insert id="insertXml" useGeneratedKeys="true" keyProperty="id">
        <selectKey keyProperty="id" order="AFTER" resultType="int">
            select max(id) + 1 as id from tbl_memo
        </selectKey>
        insert into tbl_memo (id, text) values (#{id}, #{text})
    </insert>

    <update id="updateXml">
        update tbl_memo set text=#{text} where id=#{id}
    </update>
    
    <delete id="deleteXml">
        delete from tbl_memo where id=#{id}
    </delete>
    
    <select id="selectAtXml" resultType="com.example.app.domain.dto.MemoDto" parameterType="int">
        select * from tbl_memo where id=#{id}
    </select>
    
    <select id="selectAllXml" resultType="com.example.app.domain.dto.MemoDto">
        select * from tbl_memo
    </select>
    
    <select id="selectAllResultMapXml" resultType="java.util.Map">
        select * from tbl_memo
    </select>

</mapper>
```

---

## 🔍 코드 분석

### ✅ 매퍼 네임스페이스

- `namespace="com.example.app.domain.mapper.MemoMapper"`  
    이 매퍼는 `MemoMapper` 인터페이스와 연결되어 사용됨. Mapper Interface와 이 XML 파일이 1:1 매핑됨.
    

---

### ✅ `insertXml`

```xml
<insert id="insertXml" useGeneratedKeys="true" keyProperty="id">
    <selectKey keyProperty="id" order="AFTER" resultType="int">
        select max(id) + 1 as id from tbl_memo
    </selectKey>
    insert into tbl_memo (id, text) values (#{id}, #{text})
</insert>
```

- `insertXml`: 메모 데이터 삽입용 SQL
    
- `selectKey`: `id` 값을 직접 설정. DB의 auto_increment 대신 사용
    
    - `order="AFTER"`: insert 이후 실행
        
    - `select max(id)+1`: 기존 ID 중 가장 큰 값에 1을 더해 새 ID 생성 (주의: 동시성 문제 발생 가능)
        
- `useGeneratedKeys`, `keyProperty`: 자바 객체의 `id` 필드를 자동 설정함
    

---

### ✅ `updateXml`

```xml
<update id="updateXml">
    update tbl_memo set text=#{text} where id=#{id}
</update>
```

- ID에 해당하는 메모의 텍스트를 수정
    

---

### ✅ `deleteXml`

```xml
<delete id="deleteXml">
    delete from tbl_memo where id=#{id}
</delete>
```

- 해당 ID를 가진 메모 삭제
    

---

### ✅ `selectAtXml`

```xml
<select id="selectAtXml" resultType="com.example.app.domain.dto.MemoDto" parameterType="int">
    select * from tbl_memo where id=#{id}
</select>
```

- 단일 메모 조회
    
- 결과는 MemoDto로 매핑됨
    

---

### ✅ `selectAllXml`

```xml
<select id="selectAllXml" resultType="com.example.app.domain.dto.MemoDto">
    select * from tbl_memo
</select>
```

- 모든 메모 조회 (리스트 형태로 반환)
    
- 결과는 MemoDto로 매핑됨
    

---

### ✅ `selectAllResultMapXml`

```xml
<select id="selectAllResultMapXml" resultType="java.util.Map">
    select * from tbl_memo
</select>
```

- 모든 메모를 Map 형태로 조회
    
    - key: 컬럼명, value: 값
        
    - DTO 없이 데이터를 가공할 때 유용함
        

---

## 📌 요약

- `MemoMapper.xml`은 메모 관련 CRUD 기능을 정의한 MyBatis 매퍼 파일
    
- `selectKey`로 `id`를 수동으로 생성 (주의: 동시성 문제)
    
- `resultType`으로 DTO 클래스 또는 Map 지정 가능
    
- `insertXml`, `updateXml`, `deleteXml`, `selectAtXml`, `selectAllXml`, `selectAllResultMapXml` 총 6개의 쿼리 정의됨
    
