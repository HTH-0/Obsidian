# 📄 JSP에서 EL(Expression Language) 사용 예제 정리

## 제목(한글 + 영어): JSP EL(Expression Language) 기본 사용법 정리

## 📌 개념 요약

EL(Expression Language)은 JSP에서 자바 코드를 줄이고, 속성(attribute) 값을 간편하게 출력하기 위해 사용하는 표현식 언어

---

## ✅ 주요 내용

- `${param.name}` : `request.getParameter("name")`과 동일
    
- `${scopeName.key}` : 지정된 스코프에서 key 값 접근 (예: `${sessionScope.user}`)
    
- EL 기본 검색 우선순위: **page → request → session → application**
    
- `${empty var}` : null 또는 비어 있는 값인지 확인하는 연산자
    
- EL에서는 `+`, `-`, `*`, `/` 등의 기본 연산자 사용 가능
    
- EL은 자바빈(JavaBean) 객체의 getter를 자동으로 호출 (`${userDto.userid}` → `userDto.getUserid()`)
    

---

## 💻 코드 예시 (JSP)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="C04.UserDto" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>EL 사용 예제</title>
</head>
<body>

<%
	// 각 Scope에 속성 저장
	pageContext.setAttribute("P_ATTR", "P_ATTR_VALUE");
	request.setAttribute("R_ATTR", "R_ATTR_VALUE");
	session.setAttribute("S_ATTR","S_ATTR_VALUE");
	application.setAttribute("A_ATTR","A_ATTR_VALUE");

	// 중복 키 등록 (우선순위 확인용)
	pageContext.setAttribute("DUP", "PAGE VALUE");
	request.setAttribute("DUP", "REQUEST VALUE");
	session.setAttribute("DUP","SESSION VALUE");
	application.setAttribute("DUP","APPLICATION VALUE");
%>

<hr />
<!-- EL : PARAM (요청 파라미터 출력) -->
USERNAME : ${param.username} <br />
PASSWORD : ${param.password} <br />

<!-- EL : 각 Scope의 Attribute 출력 -->
PAGECONTEXT's ATTR : ${P_ATTR} <br />
PAGECONTEXT's ATTR : ${pageScope.P_ATTR} <br />
REQUESTCONTEXT's ATTR : ${R_ATTR} <br />
REQUESTCONTEXT's ATTR : ${requestScope.R_ATTR} <br />
SESSION's ATTR : ${S_ATTR} <br />
SESSION's ATTR : ${sessionScope.S_ATTR} <br />
APPLICATION's ATTR : ${A_ATTR} <br />
APPLICATION's ATTR : ${applicationScope.A_ATTR} <br />

<hr />
<!-- EL : 중복 키 테스트 (우선순위 page → request → session → application) -->
DUPLICATED VALUE : ${DUP} <br />
<hr />

<!-- EL : 객체 속성 출력 -->
<%
	UserDto userDto = new UserDto("user1", "1234", "ROLE_USER");
	request.setAttribute("userDto", userDto);
%>
표현식 : <%= userDto.getUserid() %> <br />
EL : ${userDto.userid} <br />

<!-- EL : 연산자 -->
연산 (스크립틀릿): <%= 1+1 %> <br />
연산 (EL): ${1+1} <br />

<!-- EL : null 체크 -->
NULL : ${null} <br />
empty NULL : ${empty null} <br />
empty TEST(존재X) : ${empty TEST} <br />
empty not null : ${!empty TEST} <br />

</body>
</html>
```

---

## 📌 정리

- `${param.key}` → request 파라미터 접근
    
- `${scopeName.key}` 또는 `${key}` → 스코프 우선순위 따라 attribute 접근
    
- `${empty var}` → null, 빈 문자열 등 체크할 때 유용
    
- EL 사용 시 getter 메서드 자동 호출 (JavaBean 규약 필수)
    
- 스코프 중복 키는 **page → request → session → application** 순서로 찾음
    

---

## 🔎 추가 개념

- EL에서 JavaBean 객체 접근 시, `getXxx()` 또는 `isXxx()` 형식 필수
    
- EL은 자바 코드 삽입 없이 표현을 간결하게 함
    
- 단순 출력에는 EL을, 복잡한 로직에는 JSTL 또는 서블릿에서 처리 후 전달하는 방식이 권장됨
    

---