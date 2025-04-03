
---

# 📄 JSP 페이지 분할 구성 (`TopHeader.jsp`, `Nav.jsp`, `Footer.jsp` 사용)

## 📌 개념 요약

JSP에서 공통 레이아웃(헤더, 네비게이션, 푸터 등)을 별도 파일로 분리하고 `<%@ include file="..." %>`를 통해 재사용할 수 있음.

---

## ✅ 주요 내용

- `TopHeader.jsp` : 상단 로고, 제목, 스타일 등 구성
    
- `Nav.jsp` : 네비게이션 메뉴 담당
    
- `Footer.jsp` : 사이트 하단 정보 표시
    
- `index.jsp` : 위의 공통 구성 파일들을 포함하는 메인 페이지
    
- `<%@ include file="..." %>` : 컴파일 시 JSP를 그대로 삽입 (정적 include)
    

---

## 💻 코드 예시

### 📎 index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<%@ include file="TopHeader.jsp" %>
<%@ include file="Nav.jsp" %>

<!-- 실제 페이지 콘텐츠 -->
<div class="content">
    <h1>메인 콘텐츠 영역</h1>
    <p>여기에 각 JSP별 본문 콘텐츠가 들어감</p>
</div>

<%@ include file="Footer.jsp" %>
```

### 📎 TopHeader.jsp

```jsp
<div class="header">
    <h1>My JSP Web Site</h1>
    <hr>
</div>
```

### 📎 Nav.jsp

```jsp
<div class="nav">
    <a href="index.jsp">Home</a> |
    <a href="about.jsp">About</a> |
    <a href="contact.jsp">Contact</a>
    <hr>
</div>
```

### 📎 Footer.jsp

```jsp
<div class="footer">
    <hr>
    <p>&copy; 2025 My JSP Website. All rights reserved.</p>
</div>
```

---

## 📌 정리

- 공통 UI를 JSP 파일로 분리해 재사용 가능
    
- `<%@ include file="..." %>`는 정적 include 방식 → 코드 재사용 및 유지보수 편리
    
- 다른 방식으로는 `<jsp:include>` 동적 include도 있음 (실행 시점 삽입)
    

---

## 🔎 추가 개념

- 정적 include (`<%@ include ... %>`)는 컴파일 전에 삽입됨
    
- 동적 include (`<jsp:include page="..." />`)는 실행 중에 동작하며 파라미터 전달이 가능
    
- 분리된 구성은 유지보수, 협업, 디자인 변경 시 효율적
    

---
