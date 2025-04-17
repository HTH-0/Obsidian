# 📁 web.xml 리스너 설정 정리

---

## 📄 web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" 
         version="4.0">

  <display-name>07LISTENER</display-name>

  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
  <listener>
    <listener-class>Listener.C01ServletContextListener</listener-class>
  </listener>

</web-app>
```

---

## 🧠 코드 설명

- `<display-name>`  
    → 프로젝트 이름 또는 WAR 이름  
    → 콘솔 로그나 WAS 콘솔에서 애플리케이션 식별용
    
- `<welcome-file-list>`  
    → 클라이언트가 `/`로 접근할 경우 자동으로 보여줄 기본 파일 리스트 지정  
    → 순서대로 탐색되며, 가장 먼저 존재하는 파일이 렌더링됨
    
- `<listener>`  
    → `C01ServletContextListener` 클래스를 명시적으로 등록  
    → `@WebListener` 어노테이션 없이도 리스너 등록 가능 (web.xml 방식)  
    → 서버 시작/종료 시점에 리스너의 `contextInitialized`, `contextDestroyed` 메서드 실행됨
    

---

✅ 주의:  
다른 리스너들은 `@WebListener`로 등록돼 있으므로 web.xml에서 따로 명시하지 않아도 자동 인식됨.  
다만 `C01ServletContextListener`는 어노테이션 주석 처리되어 있으므로, `web.xml`에서 직접 등록한 것.
