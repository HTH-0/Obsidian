# 🧪 MybatisTests.java - MyBatis 단위 테스트 상세 정리

## 📦 전체 코드 (불필요한 import 제거)

```java
package DbTests;

@ExtendWith(SpringExtension.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
class MybatisTests {

	@Autowired
	private SqlSessionFactory sqlSessionFactory;
	
	@Autowired
	private MemoMapper memoMapper;

	@Test
	@Disabled
	void t1() {
		// SqlSessionFactory 빈이 정상적으로 주입되었는지 테스트
		assertNotNull(sqlSessionFactory);

		// SqlSession 객체를 열어서 정상 생성 여부 테스트
		SqlSession sqlSession =  sqlSessionFactory.openSession();
		assertNotNull(sqlSession);	
	}
	
	@Test
	void t2() {
		// 1. 어노테이션 방식 insert 테스트
//		memoMapper.insert(new MemoDto(1010, "a", "a@naver.com", LocalDateTime.now(), null));

		// 2. 어노테이션 방식 update 테스트
//		memoMapper.update(new MemoDto(1010, "qbqbqbqb", "a@naver.com", LocalDateTime.now(), null));

		// 3. 어노테이션 방식 delete 테스트
//		memoMapper.delete(1);

		// 4. 어노테이션 방식 select (단건 조회)
//		System.out.println(memoMapper.selectAt(111));

		// 5. 어노테이션 방식 selectAll (전체 DTO 리스트 조회)
//		List<MemoDto> list = memoMapper.selectAll();
//		list.forEach(System.out::println);

		// 6. 어노테이션 방식 ResultMap을 활용한 전체 Map 조회
//		List<Map<String,Object>> list = memoMapper.selectAllResultMap();
//		list.forEach(System.out::println);

		// 7. XML 방식 insert 테스트
//		memoMapper.insertXml(new MemoDto(2022, "b", "b@naver.com", LocalDateTime.now(), null));

		// 8. XML 방식 ResultMap을 활용한 전체 Map 조회
//		List<Map<String,Object>> list = memoMapper.selectAllResultMapXml();
//		list.forEach(System.out::println);

		// 9. 실제 insert 테스트 (id는 null, @SelectKey로 자동생성됨)
		MemoDto dto = new MemoDto(null, "a11", "a@naver.com", LocalDateTime.now(), null);
		memoMapper.insert(dto);
		System.out.println("Result : " + dto);
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 설정

- `@ExtendWith(SpringExtension.class)`: JUnit5에서 스프링 테스트 컨텍스트 사용
    
- `@ContextConfiguration(...)`: 스프링 빈 설정 파일 명시 (`root-context.xml`)
    

---

### ✅ 의존성 주입 (DI)

- `SqlSessionFactory`: MyBatis 세션 팩토리
    
- `MemoMapper`: 메모 DB 연동 인터페이스, 자동 구현되어 주입됨
    

---

### ✅ 테스트 메서드별 기능 정리

#### `t1()` - SQL 세션 팩토리 동작 확인

- `assertNotNull(sqlSessionFactory)`: 빈 주입 확인
    
- `sqlSessionFactory.openSession()`: 수동 세션 생성 가능 여부 확인
    

---

#### `t2()` - 각 기능별 단위 테스트 모음

|기능|설명|
|---|---|
|1. `insert()`|어노테이션 기반으로 메모 삽입|
|2. `update()`|메모 ID 기준으로 텍스트 수정|
|3. `delete()`|특정 ID의 메모 삭제|
|4. `selectAt()`|단일 메모 조회|
|5. `selectAll()`|전체 메모 조회 (`List<MemoDto>`)|
|6. `selectAllResultMap()`|전체 메모를 Map 형태로 조회 (`List<Map<String, Object>>`)|
|7. `insertXml()`|XML 매퍼 기반 삽입|
|8. `selectAllResultMapXml()`|XML 매퍼 기반 Map 리스트 조회|
|9. `insert()` with `@SelectKey`|ID는 null로 주고, max(id)+1 로 자동 세팅된 insert 테스트|

---

## 📌 요약

- 이 테스트 클래스는 어노테이션 방식과 XML 방식 모두에 대해 CRUD 기능을 개별적으로 검증할 수 있음
    
- `@SelectKey`를 활용한 id 자동 생성도 포함되어 있음
    
- 각 테스트는 주석 해제만으로 쉽게 개별 실행 가능함
    
- 테스트 흐름을 따라가며 MyBatis와 스프링 연동 상태를 빠르게 확인 가능함
    

필요 시 `MemoDto` 구조나 실제 DB 테이블 정의도 함께 정리 가능함.