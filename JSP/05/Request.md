
---

# 🧾 JSP Request 객체 정보 출력 예제 분석

## ✅ 주요 목적

- `request` 내장 객체에서 제공하는 메서드/헤더를 통해
    
    - **요청 프로토콜**
        
    - **클라이언트 주소 정보**
        
    - **서버 정보**
        
    - **요청 방식, URI, 브라우저 정보** 등 추출
        

---

## 💻 코드 분석

### 🔹 클라이언트와 서버 정보

```jsp
String protocol = request.getProtocol();           // ex: HTTP/1.1
String serverName = request.getServerName();       // ex: localhost
int serverPort = request.getServerPort();          // ex: 8080
String removeAddr = request.getRemoteAddr();       // 클라이언트 IP
String remoteHost = request.getRemoteHost();       // 클라이언트 호스트 이름
```

|항목|설명|예시|
|---|---|---|
|`getProtocol()`|요청 프로토콜|`HTTP/1.1`|
|`getServerName()`|서버 이름|`localhost` 또는 도메인|
|`getServerPort()`|요청 받은 포트|`8080`|
|`getRemoteAddr()`|클라이언트 IP 주소|`127.0.0.1`|
|`getRemoteHost()`|클라이언트 호스트 이름|`localhost` (또는 IP 기반)|

---

### 🔹 요청 관련 정보

```jsp
String method = request.getMethod();               // GET 또는 POST
StringBuffer requestURL = request.getRequestURL(); // 전체 URL
String requestURI = request.getRequestURI();       // URI 부분
```

|항목|설명|예시|
|---|---|---|
|`getMethod()`|요청 방식|`GET` 또는 `POST`|
|`getRequestURL()`|전체 요청 URL|`http://localhost:8080/myweb/info.jsp`|
|`getRequestURI()`|URL 중 URI 부분|`/myweb/info.jsp`|

---

### 🔹 클라이언트 브라우저 정보

```jsp
String Browser = request.getHeader("User-Agent");
String fileType = request.getHeader("Accept");
```

|항목|설명|예시|
|---|---|---|
|`User-Agent`|브라우저 정보|`Mozilla/5.0 (Windows NT...)`|
|`Accept`|클라이언트가 수용 가능한 MIME 타입|`text/html,application/xhtml+xml,...`|

---

## 📄 HTML 출력 결과 예시

```html
프로토콜 : HTTP/1.1  
서버이름 : localhost  
서버포트 : 8080  
고객주소 : 127.0.0.1  
고객이름 : localhost  
요청함수 : GET  
요청경로 : http://localhost:8080/myweb/info.jsp  
요청경로 : /myweb/info.jsp  
브라우저 : Mozilla/5.0 (Windows NT 10.0; Win64; x64)...  
파일타입 : text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8  
```

---

## 📌 요약

|메서드|설명|
|---|---|
|`request.getProtocol()`|HTTP 프로토콜 버전 반환|
|`request.getServerName()`|요청받은 서버의 이름 (도메인)|
|`request.getServerPort()`|요청받은 포트 번호|
|`request.getRemoteAddr()`|클라이언트 IP 주소|
|`request.getRemoteHost()`|클라이언트 호스트 이름|
|`request.getMethod()`|요청 방식 (`GET`, `POST`)|
|`request.getRequestURL()`|전체 요청 URL|
|`request.getRequestURI()`|URI 경로 부분|
|`request.getHeader("User-Agent")`|브라우저 정보|
|`request.getHeader("Accept")`|지원 MIME 타입|

---

## 🔎 실전 활용 팁

- 로그 기록용: 요청 정보, 클라이언트 정보 추적
    
- 브라우저 타입에 따라 분기 처리 가능 (구형 브라우저 대응)
    
- IP 기반 사용자 통계 또는 보안 처리에 활용 가능
    

---
