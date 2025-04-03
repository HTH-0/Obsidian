
---

# 📁 JSP 파일 다운로드 처리 예제 분석

## ✅ 전체 동작 요약

1. 서버의 특정 디렉토리에서 `TEST.txt` 파일을 읽어옴
    
2. 응답 헤더를 통해 **다운로드로 처리**되도록 설정
    
3. `ServletOutputStream`으로 파일 내용을 전송
    
4. 브라우저는 파일을 저장하도록 팝업을 띄움
    

---

## 🧩 주요 코드 분석

### 1️⃣ **파일 경로 설정**

```jsp
String dirPath = pageContext.getServletContext().getRealPath("/") + "C05\\files\\";
```

- `getRealPath("/")`는 웹 루트 경로 반환 (예: `C:/Tomcat/webapps/myweb/`)
    
- 그 아래 `C05/files/` 폴더에 있는 `TEST.txt`를 타겟으로 지정
    
- 콘솔로 실제 경로를 확인할 수 있음
    

---

### 2️⃣ **스트림 준비 (InputStream/OutputStream)**

```jsp
FileInputStream in = new FileInputStream(dirPath + "TEST.txt");
ServletOutputStream bout = response.getOutputStream();
```

- `in`: 서버의 파일을 바이트 단위로 읽음
    
- `bout`: 클라이언트로 데이터를 보낼 OutputStream
    

---

### 3️⃣ **출력 스트림 초기화**

```jsp
out.clear(); // JSPWriter의 버퍼 비움
out = pageContext.pushBody(); // JSPWriter 재연결 방지
```

- JSP 기본 출력 객체 `out`은 `PrintWriter`인데, 이걸 쓰면 **바이너리 출력 충돌 발생**
    
- 따라서 `out` 버퍼 비우고 `pushBody()`로 연결 끊고, **`ServletOutputStream` 사용**
    

💡 `getWriter()`와 `getOutputStream()`은 **동시 사용 불가** → 여기선 바이너리 전송 위해 `getOutputStream()`만 사용

---

### 4️⃣ **다운로드 응답 헤더 설정**

```jsp
response.setHeader("Content-Type", "application/octet-stream;charset=utf-8");
response.setHeader("Content-Disposition", "attachment; filename=TEST.txt");
```

- `Content-Type`: 바이너리 파일 의미 (`application/octet-stream`)
    
- `Content-Disposition`:
    
    - `attachment`: 다운로드 팝업 유도
        
    - `filename=...`: 저장 시 파일 이름 지정
        

💡 브라우저는 이 헤더를 보고 **"파일 다운로드" 동작으로 인식**함

---

### 5️⃣ **파일 전송**

```jsp
byte[] buff = new byte[4096];
while(true){
    int data = in.read(buff);
    if(data == -1) break;
    bout.write(buff, 0, data);
    bout.flush();
}
```

- 4KB씩 읽어서 `bout`으로 클라이언트에 전송
    
- 반복 종료 후 스트림 닫음
    

---

## 📌 출력 결과 (브라우저 동작)

- 페이지에 아무것도 출력되지 않고,
    
- 브라우저는 자동으로 `TEST.txt` 다운로드 창이 뜸
    

---

## ⚠️ 주의 사항

|주의 포인트|설명|
|---|---|
|`out.clear()`|JSPWriter와 충돌 방지|
|`getOutputStream()` 단독 사용|`getWriter()` 와 함께 사용하면 오류|
|파일 경로는 반드시 절대경로|상대경로로 하면 `FileNotFoundException` 가능성|
|Content-Type 헤더 정확히 설정|잘못되면 브라우저가 페이지로 오해함|

---

## 🛠 실무 팁

- 한글 파일 이름 처리 시 `URLEncoder.encode(...)`로 인코딩 필요
    
- 파일이 없는 경우 `404`, `sendError(...)`로 안내 처리 가능
    
- 서버 외부 경로(`C:/upload/...`)도 활용 가능하지만, 보안 설정 필요
    

---

## ✨ 파일 다운로드에 적합한 JSP 구조 예시

```jsp
<%
response.setContentType("application/octet-stream");
response.setHeader("Content-Disposition", "attachment; filename=파일명");

try (FileInputStream in = new FileInputStream(filePath);
     ServletOutputStream out = response.getOutputStream()) {

    byte[] buffer = new byte[4096];
    int length;
    while ((length = in.read(buffer)) != -1) {
        out.write(buffer, 0, length);
    }
}
%>
```

---

