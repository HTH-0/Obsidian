
---

# ✅ SpringBoot + MyBatis + HikariCP + Mapper 통합 정리

---

## 1️⃣ 테이블 구조 (`tbl_memo`)

🔧 모든 DTO, Mapper, XML, 테스트 기준 아래와 같이 구성됨:

```sql
CREATE TABLE tbl_memo (
    id INT PRIMARY KEY,
    text VARCHAR(255),
    writer VARCHAR(100),
    createAt DATETIME
);
```

- **id**: @SelectKey 또는 selectKey로 `max(id)+1` 자동 설정
    
- **text**: 메모 본문
    
- **writer**: 이메일 형식의 작성자
    
- **createAt**: 생성 시각
    
- `dateTest`는 DB와 매핑되지 않음 (LocalDate)
    

---

## 2️⃣ DTO 클래스

### 📂 `MemoDto.java`

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class MemoDto {
    @Min(10)
    @Max(65535)
    @NotNull
    private Integer id;

    @NotBlank
    private String text;

    @NotBlank
    @Email
    private String writer;

    @DateTimeFormat(pattern="yyyy-MM-dd'T'HH:mm")
    @NotNull
    private LocalDateTime createAt;

    private LocalDate dateTest; // DB 비매핑 필드
}
```

> ✅ 유효성 검사 포함: `@Min`, `@Max`, `@NotBlank`, `@Email`

---

## 3️⃣ Mapper 구성

### 📂 `MemoMapper.java` (Java 어노테이션 방식)

```java
@Mapper
public interface MemoMapper {

    @SelectKey(statement="select max(id)+1 from tbl_memo", keyProperty="id", before=false, resultType=int.class)
    @Insert("insert into tbl_memo values(#{id},#{text},#{writer},#{createAt})")
    int insert(MemoDto memoDto);

    @Update("update tbl_memo set text=#{text} where id=#{id}")
    int update(MemoDto dto);

    @Delete("delete from tbl_memo where id=#{id}")
    int delete(int id);

    @Select("select * from tbl_memo where id=#{id}")
    MemoDto selectAt(int id);

    @Select("select * from tbl_memo")
    List<MemoDto> selectAll();

    @Select("select * from tbl_memo")
    @Results(id="MemoResultMap", value={
        @Result(property="id", column="id"),
        @Result(property="text", column="text")
    })
    List<Map<String,Object>> selectAllResultMap();

    // XML 방식
    int insertXml(MemoDto memoDto);
    int updateXml(MemoDto memoDto);
    int deleteXml(@Param("id") int id);
    MemoDto selectAtXml(int id);
    List<MemoDto> selectAllXml();
    List<Map<String,Object>> selectAllResultMapXml();
    List<Map<String,Object>> Select_if_xml(Map<String,Object> param);
    List<Map<String,Object>> Select_when_xml(Map<String,Object> param);
}
```

---

### 📂 `MemoMapper.xml` (XML 방식 매핑)

```xml
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

<select id="selectAtXml" resultType="com.example.demo.domain.dto.MemoDto">
    select * from tbl_memo where id=#{id}
</select>

<select id="Select_if_xml" resultType="java.util.Map">
    select * from tbl_memo
    <if test="type!=null and type.equals('text')">
        where text like concat('%',#{keyword},'%')
    </if>
</select>
```

---

## 4️⃣ 테스트 코드

### 📂 `MemoMapperTest.java`

```java
@SpringBootTest
@MapperScan
class MemoMapperTest {

    @Autowired
    private MemoMapper memoMapper;

    @Test
    public void t1() {
        MemoDto dto = new MemoDto(444, "a", "a", LocalDateTime.now(), null);
        memoMapper.insert(dto); // 어노테이션 기반
    }

    @Test
    public void t2() {
        MemoDto dto = new MemoDto(555, "a", "a", LocalDateTime.now(), null);
        memoMapper.insertXml(dto); // XML 기반
    }
}
```

---

## ✅ 요약

|구성요소|설명|
|---|---|
|DataSource|HikariCP (수동 등록)|
|MyBatis 설정|SqlSessionFactory 수동 등록 + XML 매퍼 설정|
|Mapper 방식|어노테이션 & XML 병행|
|DTO|유효성 검사 포함 + LocalDateTime 지원|
|테이블|`tbl_memo(id, text, writer, createAt)`|
|테스트|insert, select 테스트 모두 구현 완료|

---
