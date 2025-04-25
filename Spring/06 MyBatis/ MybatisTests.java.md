# 🧪 MybatisTests 테스트 클래스 분석

## 📦 전체 코드

```java
package DbTests;

import static org.junit.jupiter.api.Assertions.assertNotNull;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.junit.jupiter.api.Disabled;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit.jupiter.SpringExtension;

import com.example.app.domain.mapper.MemoMapper;

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
		assertNotNull(sqlSessionFactory);
		SqlSession sqlSession =  sqlSessionFactory.openSession();
		assertNotNull(sqlSession);	
	}
	
	@Test
	void t2() {
//		memoMapper.insert(new MemoDto(1010,"a","a@naver.com",LocalDateTime.now(),null));
//		memoMapper.update(new MemoDto(1010,"qbqbqbqb","a@naver.com",LocalDateTime.now(),null));
//		memoMapper.delete(1);
//		System.out.println(memoMapper.selectAt(111));
		
//		List<MemoDto> list = memoMapper.selectAll();
//		list.forEach(System.out::println);
		
//		List<Map<String,Object>>list =  memoMapper.selectAllResultMap();
//		list.forEach(System.out::println);
		
//		memoMapper.insertXml(new MemoDto(2022,"b","b@naver.com",LocalDateTime.now(),null));
		
//		List<Map<String,Object>> list = memoMapper.selectAllResultMapXml();
//		list.forEach(System.out::println);
//		MemoDto dto = new MemoDto(null, "a11", "a@naver.com", LocalDateTime.now());
//		memoMapper.insert(dto);
//		System.out.println("Result : " + dto);
	}
	
	@Test
	void t3() {
		Map<String, Object> param = new HashMap<>();
		param.put("type", "writer");
		param.put("keyword", "TEST");
		
//		List<Map<String, Object>> response = memoMapper.Select_if_xml(param);
//		response.forEach(System.out::println);
		
		List<Map<String, Object>> response = memoMapper.Select_when_xml(param);
		response.forEach(System.out::println);
	}
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@ExtendWith(SpringExtension.class)`  
    → JUnit5에서 스프링 테스트 기능 사용 가능하게 설정
    
- `@ContextConfiguration(...)`  
    → `root-context.xml`을 통해 Spring Bean 설정 파일 로드
    

### ✅ 의존성 주입 (DI)

- `@Autowired private SqlSessionFactory sqlSessionFactory;`  
    → MyBatis 설정을 통해 생성된 SqlSessionFactory 자동 주입
    
- `@Autowired private MemoMapper memoMapper;`  
    → 인터페이스 기반의 Mapper를 MyBatis가 구현체로 자동 주입
    

### ✅ 주요 테스트 메서드 설명

#### 🧪 `t1()`

- MyBatis 설정이 제대로 되었는지 확인
    
- `sqlSessionFactory`, `sqlSession` 객체가 null이 아닌지 검증
    
- `@Disabled` 처리되어 실행되지 않음 (MyBatis 연결 테스트용으로만 존재)
    

#### 🧪 `t2()`

- 다양한 `memoMapper` 메서드를 테스트하기 위한 구간
    
- 주석처리된 테스트 목록:
    
    - `insert`, `update`, `delete`, `selectAt`, `selectAll`, `selectAllResultMap`
        
    - `insertXml`, `selectAllResultMapXml`
        
- 실제로는 주석 처리되어 있으나, 이 영역은 기능별 테스트를 수동으로 해볼 때 사용하는 부분
    

#### 🧪 `t3()`

- 조건 검색 관련 테스트
    
- `param` 맵에 `"type": "writer"` 와 `"keyword": "TEST"` 조건을 넣어 전달
    
- `Select_when_xml()` 메서드 실행 결과를 출력
    
    - 주석 처리된 `Select_if_xml()` 과 비교하면서 `<if>`, `<choose>` 조건 처리 테스트 가능
        

---

## 📌 요약

- `MybatisTests`는 MyBatis와 Spring 연동 테스트를 수행하는 JUnit 클래스
    
- 각 테스트 메서드는 MyBatis 기능(연결, CRUD, 조건 검색 등)을 독립적으로 검증
    
- `memoMapper`는 `MemoMapper.xml`과 연결된 매퍼 인터페이스로, 다양한 메서드를 통해 SQL 실행
    
- `t2()`는 실제 비즈니스 로직을 테스트할 때 수동으로 주석 해제해 사용하는 테스트 슬롯 역할
    
- `t3()`는 `<if>`, `<when>` 조건 검색 SQL 매퍼의 테스트 확인용
    
