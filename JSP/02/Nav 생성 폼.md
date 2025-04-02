# 📄 사용자 입력 기반 동적 Nav 생성 폼 (JSP 입력 예제)

## 📌 개념 요약

사용자가 입력한 열 이름(`col1`, `col2`, ...)과 스타일 코드(`style`)를 기반으로 동적인 네비게이션 바를 생성할 수 있도록 데이터를 `C05Result.jsp`로 전달하는 폼 구성

---

## ✅ 주요 내용

- `form`의 `action="C05Result.jsp"`: 데이터를 전송할 JSP 지정
    
- 입력 필드 이름은 `col1`, `col2`, `col3`, `col4`, `style`
    
- 사용자가 입력한 값을 다음 JSP에서 받아 네비게이션 UI를 동적으로 생성
    
- `style` 필드를 통해 사용자 정의 CSS 속성 적용 가능
    

---

## 💻 코드 예시 (HTML Form)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<!-- NAV 항목과 스타일 전달 폼 -->
	<form action="C05Result.jsp">
		<h2>NAV 만들기</h2>
		
		<!-- 열 이름 입력 -->
		<input name="col1" placeholder="열이름1">
		<input name="col2" placeholder="열이름2">
		<input name="col3" placeholder="열이름3">
		<input name="col4" placeholder="열이름4">
		
		<!-- 스타일 코드 입력 -->
		<input name="style" placeholder="Nav기본 스타일Code">

		<!-- 전송 -->
		<button>페이지 생성 요청</button>
	</form>	

</body>
</html>
```

---

## 📌 정리

- 사용자가 작성한 메뉴 항목(`col1 ~ col4`)과 스타일(`style`) 값을 `C05Result.jsp`에서 받아서 동적 `<nav>` 구성
    
- 입력값은 `request.getParameter()` 또는 `${param.col1}` 등으로 수신 가능
    
- `style`은 `<nav style="${param.style}">`처럼 직접 적용 가능
    

---

## 🔎 추가 개념

- 네비게이션 항목은 `<a href="#">${param.col1}</a>` 형태로 반복 출력 가능
    
- 스타일 입력 예시: `background:lightgray; padding:10px; display:flex; gap:10px;`
    
- 나중에 `JSTL`이나 `EL`을 활용한 반복 출력 구성으로 확장 가능
    

---