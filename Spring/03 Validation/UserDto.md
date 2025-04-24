# 👤 UserDto 유효성 검사 DTO

## 📦 전체 코드

```java
package com.example.app.domain.Dto;

import java.time.LocalDate;

import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;

import org.springframework.format.annotation.DateTimeFormat;

import lombok.Data;

@Data
public class UserDto {
	private String userid;		//유저ID

	@NotBlank(message="password 를 입력하세요")
	private String password;	//패스워드

	@NotBlank(message="rePassword 를 입력하세요")
	private String rePassword;	//패스워드확인

	@NotBlank(message="username 를 입력하세요")
	private String username;	//유저이름

	@NotBlank(message="phone 를 입력하세요")
	private String phone;		//전화번호

	@NotBlank(message="zipcode 를 입력하세요")
	private String zipcode;		//우편번호

	@NotBlank(message="addr1 를 입력하세요")
	private String addr1;		//기본주소

	@NotBlank(message="addr2 를 입력하세요")
	private String addr2;		//상세주소

	@NotNull(message="birthday 를 입력하세요")
	@DateTimeFormat(pattern="yyyy-MM-dd")
	private LocalDate birthday;	//생년월일(yyyy-MM-dd)
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Data`: Lombok에서 `getter`, `setter`, `toString`, `equals`, `hashCode` 자동 생성
    
- DTO (Data Transfer Object)로 사용되는 클래스
    
- 유효성 검사를 위한 `javax.validation.constraints` 기반 어노테이션 포함
    

### ✅ 주요 필드 유효성 검사 설명

- `userid`
    
    - 유효성 검사 없음 (로그인 시 활용 가능성이 높으므로 필요에 따라 추가 가능)
        
- `password`, `rePassword`, `username`, `phone`, `zipcode`, `addr1`, `addr2`
    
    - `@NotBlank`: null, "", 공백 문자 모두 허용하지 않음
        
    - 각각의 필수 입력 항목
        
- `birthday`
    
    - `@NotNull`: null 허용하지 않음
        
    - `@DateTimeFormat(pattern="yyyy-MM-dd")`
        
        - 폼에서 전달되는 문자열을 `LocalDate`로 변환할 때의 포맷 지정
            
        - 예시 입력값: `2000-05-10`
            

---

## 📌 요약

- `UserDto`는 회원가입 시 사용하는 유효성 검사용 DTO 클래스
    
- 이름, 비밀번호, 주소, 생일 등의 필수 입력값을 `@NotBlank`, `@NotNull`로 검증
    
- `LocalDate` 타입 생년월일은 `@DateTimeFormat`을 통해 변환 포맷 지정 (`yyyy-MM-dd`)
    
- 유효성 실패 시 각 필드별 메시지를 에러로 반환 가능함 (ex. BindingResult 활용)