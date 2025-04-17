# 👤 UserDto 클래스 정리

---

## 📄 UserDto.java

```java
package Domain.Dto;

public class UserDto {
	private String username;
	private String password;
	private String role;

	public UserDto() {}

	public UserDto(String username, String password, String role) {
		this.username = username;
		this.password = password;
		this.role = role;
	}

	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}

	public String getRole() {
		return role;
	}
	public void setRole(String role) {
		this.role = role;
	}

	@Override
	public String toString() {
		return "UserDto [username=" + username + ", password=" + password + ", role=" + role + "]";
	}
}
```

---

## 🧠 설명

|항목|설명|
|---|---|
|`username`|사용자 아이디 (식별자)|
|`password`|비밀번호|
|`role`|권한 (ex. ROLE_USER, ROLE_ADMIN 등)|

### ✅ 특징

- DTO(Data Transfer Object)로, 계층 간 데이터 전달용
    
- 기본 생성자 + 필드 초기화 생성자 제공
    
- 표준 Getter/Setter 및 `toString()` 구현
    

---
