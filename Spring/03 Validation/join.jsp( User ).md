# 📄 회원가입 폼 JSP 페이지

## 📦 전체 코드

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<form action="${pageContext.request.contextPath }/join" method="post">
		<div>
			<h1>회원가입</h1>
		</div>
		<div>
			<label>userid : </label><span style="color:red;font-size:.7rem;">${userid}</span><br>
			<input name="userid" />
		</div>
		<div>
			<label>password : </label><span style="color:red;font-size:.7rem;">${password}</span><br>
			<input name="password" />
		</div>
		<div>
			<label>rePassword : </label><span style="color:red;font-size:.7rem;">${rePassword}</span><br>
			<input name="rePassword" />
		</div>
		<div>
			<label>username : </label><span style="color:red;font-size:.7rem;">${username}</span><br>
			<input name="username" />
		</div>
		<div>
			<label>phone : </label><span style="color:red;font-size:.7rem;">${phone}</span><br>
			<input name="phone" placeholder="0xx-xxx-xxxx or 0xx-xxxx-xxxx" />
		</div>
		<div>
			<label>zipcode : </label><span style="color:red;font-size:.7rem;">${zipcode}</span><br>
			<input name="zipcode" />
		</div>
		<div>
			<label>addr1 : </label><span style="color:red;font-size:.7rem;">${addr1}</span><br>
			<input name="addr1" />
		</div>
		<div>
			<label>addr2 : </label><span style="color:red;font-size:.7rem;">${addr2}</span><br>
			<input name="addr2" />
		</div>
		<div>
			<label>birthday : </label><span style="color:red;font-size:.7rem;">${birthday}</span><br>
			<input type="text" name="birthday" placeHolder="ex) 2025~01~01"/>
		</div>
		<div>
			<input type="submit" value="회원가입" />
		</div>
	</form>
</body>
</html>
```

---

## 🔍 코드 분석

### ✅ 전체 구조 설명

- 회원가입 양식을 구성하는 JSP 페이지
    
- `form` 태그의 `action`은 `/join` 경로로 POST 방식 전송
    
- `${pageContext.request.contextPath}` : context root를 동적으로 반영
    

### ✅ 주요 입력 필드 설명

- `userid`, `password`, `rePassword`, `username` 등 기본 사용자 정보 입력 필드
    
- `phone` : 전화번호 형식 가이드 제공 (placeholder)
    
- `zipcode`, `addr1`, `addr2` : 주소 정보 입력
    
- `birthday` : 생년월일 입력 (예시: `2025~01~01` 형식)
    

### ✅ 유효성 메시지 출력용 `<span>`

- 각 필드 옆의 `${변수명}`은 유효성 검사 실패 시 서버에서 전달받은 오류 메시지를 표시
    
- Spring에서는 `BindingResult`를 통해 에러 정보를 모델에 담아 이 JSP에서 출력 가능
    

---

## 📌 요약

- 이 JSP는 회원가입 데이터를 입력받는 폼을 구성
    
- 입력 실패 시 에러 메시지를 `<span>`에 출력
    
- 생년월일, 전화번호 등 포맷에 주의 필요
    
- 서버 측 DTO에는 `@Valid` 및 `@Pattern`, `@NotBlank` 등 검증 애노테이션 적용 예상됨