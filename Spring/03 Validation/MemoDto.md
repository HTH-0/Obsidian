# 🗒️ MemoDto 유효성 검사 DTO

## 📦 전체 코드

```java
package com.example.app.domain.Dto;

import java.time.LocalDate;
import java.time.LocalDateTime;

import javax.validation.constraints.Email;
import javax.validation.constraints.Max;
import javax.validation.constraints.Min;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;

import org.springframework.format.annotation.DateTimeFormat;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class MemoDto {
	@Min(value = 10, message = "ID는 10 이상 이어야 합니다.")
	@Max(value =65535, message ="ID는 65535 이하여야 합니다.")
	@NotNull(message = "ID는 필수항목 입니다")
	private Integer id;

	@NotBlank(message ="메모를 입력하세요")
	private String text;

	@NotBlank(message ="메모를 입력하세요")
	@Email(message="example@example.com 형식에 맞게 작성해주세요")
	private String writer;

	@DateTimeFormat(pattern = "yyyy-MM-dd'T'HH:mm")
	@NotNull(message ="날짜를 입력해주세요")
	private LocalDateTime createAt;
	
	private LocalDate dateTest;
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Data`: Lombok 제공. `getter`, `setter`, `toString`, `equals`, `hashCode` 자동 생성
    
- `@NoArgsConstructor`, `@AllArgsConstructor`: 기본 생성자 및 전체 필드 생성자 자동 생성
    
- DTO 클래스이며 `유효성 검사`와 `날짜 변환` 관련 기능 포함
    

### ✅ 주요 필드 설명 및 유효성 검사

- `private Integer id`
    
    - `@Min(10)`: 최소값 10
        
    - `@Max(65535)`: 최대값 65535
        
    - `@NotNull`: null 불가 (필수 항목)
        
- `private String text`
    
    - `@NotBlank`: 공백 문자도 허용하지 않음 ("" 불가)
        
    - 에러 메시지: "메모를 입력하세요"
        
- `private String writer`
    
    - `@NotBlank`: 공백 금지
        
    - `@Email`: 이메일 형식 유효성 검사
        
    - 이메일 형식이 아닐 경우 에러 메시지 출력
        
- `private LocalDateTime createAt`
    
    - `@DateTimeFormat(pattern = "yyyy-MM-dd'T'HH:mm")`: 문자열 → 날짜로 변환할 때 사용하는 형식 지정
        
        - 예시: `2025-04-24T11:00`
            
    - `@NotNull`: null이면 에러 발생
        
- `private LocalDate dateTest`
    
    - 별도의 제약 없음, 필요시 커스텀 에디터 등으로 포맷 지정 가능 (예: `WebDataBinder` 설정에서 사용)
        

---

## 📌 요약

- `MemoDto`는 유효성 검사를 위한 DTO 클래스
    
- 각 필드는 유효성 제약 조건이 달려 있으며, 주로 `javax.validation.constraints` 어노테이션 사용
    
- 날짜 변환은 `@DateTimeFormat`으로 지정된 포맷에 따라 처리됨
    
- `LocalDate` 필드인 `dateTest`는 커스텀 에디터 등을 통해 변환 설정 가능함 (예: `webDataBinder.registerCustomEditor(...)`)