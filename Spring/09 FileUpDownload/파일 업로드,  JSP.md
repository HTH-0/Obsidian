
---

# 🧾 파일 업로드 및 목록 JSP 정리

## 📄 `upload.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>/file/upload</h1>
	<form 
		action="${pageContext.request.contextPath}/file/upload" 
		method="post"
		enctype="multipart/form-data"
	>
		<input type="file" name="files" multiple/>
		<button> 전송 </button>
	</form>
</body>
</html>
```

### ✅ 설명

- `multipart/form-data`: 파일 업로드 시 필수 설정
    
- `<input type="file" name="files" multiple>`: 여러 파일 선택 가능
    
- 컨트롤러의 `@PostMapping("/upload")`과 연동됨
    

---

## 📄 `list.jsp`

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>/file/list</h1>
	<div>UPLOAD DIR : ${uploadPath}</div>
	
	<c:forEach items='${uploadPath.listFiles()}' var='subdir'>
		<hr />
		FOLDER : ${subdir.getPath()}
		
		<c:forEach items='${subdir.listFiles()}' var='file'>
			<br />
			- FILE : ${file.getPath()}
		</c:forEach>
	</c:forEach>
</body>
</html>
```

### ✅ 설명

- `uploadPath`는 컨트롤러에서 Model로 전달됨
    
- 각 날짜별 업로드 폴더와 그 안의 파일 목록 출력
    
- JSTL `c:forEach` 사용해 반복 출력
    

---

## 📌 요약

|JSP 파일|역할|
|---|---|
|`upload.jsp`|사용자로부터 파일 업로드를 받는 폼|
|`list.jsp`|서버에 저장된 업로드 폴더 및 파일 목록을 화면에 출력|

---
