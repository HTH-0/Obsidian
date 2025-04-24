# 📄 MEMO 등록 폼 JSP 페이지

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

	<h1>MEMO ADD(/memo/add)</h1>
	
	<form action="${pageContext.request.contextPath}/memo/add" method="post">
		<div>
			<label>id : </label> <span>${id}</span><br>
			<input name="id" />
		</div>
		<div>
			<label>text : </label>  <span>${text}</span><br>
			<textarea name="text"></textarea>
		</div>
		<div>
			<label>writer : </label>  <span>${writer}</span><br>
			<input name="writer" />
		</div>
		
		<div>
			<label>createAt : </label>  <span>${createAt}</span><br>
			<input type="datetime-local" name="createAt" />
		</div>
		
		<div>
			<label>dateTest(customFormat) : </label>  <span></span><br>
			<input type="text" name="dateTest" placeHolder="yyyy#MM#dd" />
		</div>
			
		<div>
			<input type="submit" value="메모쓰기" />
		</div>
	</form>
</body>
</html>
```

---

## 🔍 코드 분석

### ✅ 전체 구조 설명

- JSP 페이지로 메모 작성 폼을 HTML로 구성함.
    
- `action="${pageContext.request.contextPath}/memo/add"` : 서버에 `/memo/add`로 POST 요청 전송.
    
- `${pageContext.request.contextPath}`는 현재 웹 애플리케이션의 context path를 동적으로 지정.
    

### ✅ 주요 입력 필드 설명

- `id` : 숫자 입력용 텍스트 필드. 서버 DTO에서 `@Min` 유효성 검사를 수행할 가능성이 높음.
    
- `text` : 메모 내용 입력을 위한 `textarea`.
    
- `writer` : 작성자 이름을 입력하는 텍스트 필드.
    
- `createAt` : `LocalDateTime`을 위한 `datetime-local` 입력 필드.
    
    - 주의: 서버에서는 `@DateTimeFormat` 설정이 필요.
        
- `dateTest` : `yyyy#MM#dd` 형식의 날짜 입력을 위한 텍스트 필드.
    
    - 서버에서 커스텀 포맷(`#`)에 맞춰 파싱 설정 필요 (예: `@DateTimeFormat(pattern = "yyyy#MM#dd")`).
        

### ✅ 데이터 바인딩 관련 요소

- `${id}`, `${text}`, `${writer}`, `${createAt}` : 입력 실패 시 값을 다시 채우기 위한 표현.
    
- Spring에서 `Model` 또는 `BindingResult`에 값을 담아 재렌더링 시 활용.
    

---

## 📌 요약

- `/memo/add` 경로로 메모 데이터를 전송하는 폼 페이지.
    
- `id`, `text`, `writer`, `createAt`, `dateTest` 필드를 포함.
    
- 날짜 형식 입력 시 `@DateTimeFormat` 설정이 서버 측 DTO에 반드시 필요.
    
- 커스텀 날짜 포맷 입력 필드는 별도 파서 설정이 필요함.