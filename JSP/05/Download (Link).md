
---

# 📂 파일 다운로드 메뉴 페이지 (`index` 또는 `menu` 역할)

## ✅ 역할 요약

- 사용자에게 2가지 다운로드 기능을 제공
    
    - **단일 파일 다운로드 (`04Download_SingleFile.jsp`)**
        
    - **여러 파일을 압축하여 다운로드 (`05Download_zip.jsp`)**
        
- 각각의 JSP는 실제 다운로드 로직을 수행
    

---

## 💻 코드 구조 분석

```jsp
<a href="04Download_SingleFile.jsp">단일파일 다운로드</a>
<hr/>
<a href="05Download_zip.jsp">ZIP 다운로드</a>
```

### 🔹 1. 단일파일 다운로드

- **링크 클릭 시** `04Download_SingleFile.jsp` 실행
    
- 해당 파일에서 서버에 존재하는 `TEST.txt` 같은 특정 파일을 다운로드하는 로직 포함됨
    
- 사용자 브라우저는 **파일 저장 창**을 띄움
    

### 🔹 2. ZIP 다운로드

- **링크 클릭 시** `05Download_zip.jsp` 실행
    
- 서버의 특정 디렉토리에 있는 파일들을 하나의 ZIP으로 압축해 제공
    
- 사용자 브라우저는 `ALLFILES.zip` 다운로드
    

---

## 🔎 기능 흐름 요약

```plaintext
[사용자 화면]
   |
   ├─> 단일 파일 다운로드  → 04Download_SingleFile.jsp 실행 → TEST.txt 다운로드
   |
   └─> ZIP 다운로드        → 05Download_zip.jsp 실행 → ALLFILES.zip 다운로드
```

---

## 📌 UI 개선 팁 (선택사항)

간단한 CSS를 추가하면 더욱 보기 좋은 UI로 개선할 수 있어:

```jsp
<style>
  body {
    font-family: Arial;
    margin: 2em;
  }
  a {
    display: block;
    margin: 1em 0;
    font-size: 1.2em;
    text-decoration: none;
    color: #3366cc;
  }
</style>
```

---

## ✅ 결론

- 이 파일은 **다운로드 기능 진입점** 역할을 하는 간단한 HTML 메뉴 페이지
    
- 사용자는 단일/다중 다운로드 기능을 직관적으로 선택할 수 있음
    
- 실무에서는 이와 같은 페이지를 **관리자 도구, 백오피스, 자료실** 메뉴로 자주 사용함
    

---
