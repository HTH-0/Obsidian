
---

# 📦 JSP 다중 파일 압축 다운로드 (ZipOutputStream)

## ✅ 목적

- 서버에 있는 여러 파일을 ZIP으로 압축하여 클라이언트에 **한 번에 다운로드** 제공
    
- 다운로드 파일명은 `ALLFILES.zip`
    

---

## 🧩 코드 전체 분석

### 1️⃣ 디렉터리 경로 및 파일 목록 구성

```jsp
String dirPath = pageContext.getServletContext().getRealPath("/") + "C05\\files\\";
File dir = new File(dirPath);
File fileList[] = dir.listFiles(); // 디렉터리 내의 모든 파일 목록 추출
```

- `getRealPath("/")`: 현재 JSP 프로젝트의 실제 루트 경로 반환
    
- `C05/files/`: 해당 하위 디렉토리
    
- `fileList`: 모든 파일 객체를 배열로 저장
    

🔎 콘솔 출력: `System.out.println(f)`로 디버깅용 파일 확인

---

### 2️⃣ 출력 스트림 초기화

```jsp
out.clear();                  // JSP 기본 out 객체 버퍼 제거
out = pageContext.pushBody(); // JSP body에 연결 → flush 방지
ServletOutputStream bout = response.getOutputStream();
ZipOutputStream zout = new ZipOutputStream(bout);  // 최종 ZIP 스트림
```

- `ServletOutputStream`은 클라이언트로 데이터 전송을 위한 기본 바이너리 스트림
    
- 여기에 `ZipOutputStream`을 연결하여 **ZIP 파일 형식으로 전송**
    

---

### 3️⃣ 응답 헤더 설정 (중요!)

```jsp
response.setHeader("Content-Type", "application/octet-stream;charset=utf-8");
response.setHeader("Content-Disposition", "attachment; filename=ALLFILES.zip");
```

- 파일 다운로드로 처리되도록 **브라우저에게 힌트를 주는 핵심 설정**
    
- `Content-Disposition`에 `attachment`와 파일명을 지정하면, 자동 다운로드 창 뜸
    

---

### 4️⃣ ZIP 파일 생성 및 압축 처리 루프

```jsp
for(File file : fileList){
	in = new FileInputStream(file);                  // 파일 입력 스트림
	zipEntry = new ZipEntry(file.getName());         // ZIP에 추가될 항목 생성
	zout.putNextEntry(zipEntry);                     // 항목 추가
	
	while(true){
		int data = in.read();                         // 한 바이트씩 읽음
		if(data == -1) break;
		zout.write((byte)data);                       // ZIP 스트림에 씀
	}
	zout.closeEntry();                                // 현재 파일 종료
	in.close();                                       // 입력 스트림 닫기
}
```

- `ZipEntry`는 ZIP 안에 개별 파일 항목을 정의
    
- `zout.putNextEntry(...)`로 항목 시작 → 바이트 기록 → `closeEntry()`로 마무리
    

💡 한 파일씩 압축하여 하나의 ZIP으로 묶는 방식

---

### 5️⃣ 최종 정리

```jsp
zout.flush();
zout.close();
bout.close();
```

- 모든 스트림을 **flush → close** 순서로 안전하게 마무리
    

---

## 📌 최종 동작 흐름

```plaintext
[클라이언트 요청] → [JSP 실행]
→ C05/files 내 파일 목록 탐색
→ 각각을 ZipOutputStream으로 압축
→ ALLFILES.zip으로 응답 헤더 전송
→ 클라이언트 브라우저는 자동 다운로드 처리
```

---

## 💡 출력 예시 (클라이언트 관점)

- 다운로드 창에 `ALLFILES.zip` 파일 저장 요청
    
- 내부에는 서버 폴더 `C05/files/` 내의 모든 파일이 압축되어 포함됨
    

---

## ⚠️ 주의 사항 및 확장 팁

|항목|설명|
|---|---|
|파일 이름 한글 포함 시|`URLEncoder.encode(...)`로 파일명 인코딩 필요|
|`read()` 방식|현재는 **1바이트씩 처리** → 성능 개선 위해 `byte[]` 버퍼 사용 권장|
|폴더 포함 압축|현재는 폴더 구조 없이 **루트에 나열**됨|
|빈 폴더 있을 경우|ZIP에 포함되지 않음|

---

## 🛠 최적화 예시 (성능 개선)

```java
byte[] buffer = new byte[4096];
int len;
while ((len = in.read(buffer)) != -1) {
    zout.write(buffer, 0, len);
}
```

→ 1바이트씩 처리하는 것보다 4KB 단위 처리로 훨씬 빠르고 효율적

---

필요하다면:

- **압축 대상 파일 필터링** (`.txt`만 포함 등)
    
- **특정 사용자의 개인 폴더 압축**
    
- **압축 후 자동 삭제 (보안 목적)**
    

같은 고급 기능도 이어서 설명해줄 수 있어. 원할까?