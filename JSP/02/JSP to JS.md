# 📄 JSP → JavaScript 데이터 전달 (EL + JS 템플릿 리터럴)

## 📌 개념 요약

JSP에서 설정한 서버 데이터를 EL(Expression Language)을 이용해 **JavaScript로 안전하게 전달**하는 방법

---

## ✅ 주요 내용

- `<% ... %>`: JSP 스크립틀릿 블록에서 자바 변수 선언 및 `request.setAttribute()` 처리
    
- `${message}`: JSP에서 설정한 `request` 범위 속성을 EL로 출력
    
- JavaScript 내에서 EL을 통해 서버 데이터를 변수에 할당 가능
    
- JS 템플릿 리터럴(`` `...${}` ``) 문법과의 혼동 주의 → JSP와 JS 문법 충돌 조심
    

---

## 💻 코드 예시

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%
	// 백엔드 처리 영역
	String msg1 = "HELLO 1";
	String msg2 = "HELLO 2";
	String msg3 = "HELLO 3";
	String msg4 = "HELLO 4";

	request.setAttribute("message", "TEST!!");
	request.setAttribute("message2", "TEST!!_2");
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<script>
	// EL 표현식으로 JSP 데이터를 JavaScript로 전달
	const message1 = '${message}';    // requestScope.message → "TEST!!"
	const message2 = '${message2}';   // requestScope.message2 → "TEST!!_2"

	// 아래 표현식은 JS 템플릿 리터럴이 아니라 그냥 문자열
	const message3 = `${message}`;    // 주의: JS에서 ${message}는 변수 아님

	console.log(message1);   // "TEST!!"
	console.log(message2);   // "TEST!!_2"
	console.log(message3);   // 출력: `${message}`
</script>

</body>
</html>
```

---

## 📌 정리

- `${message}`: JSP 표현식, request 범위의 값 출력
    
- `<%=msg1%>`: JSP 스크립틀릿에서 변수 삽입 (비추천)
    
- JS 안에서는 EL을 문자열로 넣을 뿐, 동적으로 인식되진 않음
    
- JS의 `${}` 템플릿 리터럴과 JSP EL의 `${}`는 **완전히 별개** → 혼동 주의
    

---

## 🔎 추가 개념

- `request.setAttribute()` → 현재 요청(request)에 값을 담는 방식
    
- `<%= ... %>` vs `${...}`
    
    - `<%= %>`: JSP 출력식, 코드 삽입 느낌 (Old 방식)
        
    - `${}`: EL 표현식, 가독성 및 유지보수에 유리
        
- HTML, JS, JSP 모두 `${}` 문법을 사용하므로 **문맥에 따라 해석이 다름**
    
    - EL: JSP 서버에서 처리
        
    - JS 템플릿 리터럴: 브라우저에서 처리
        
    - 문자열 혼용 시 작은 따옴표, 백틱 구분 필수
        

---