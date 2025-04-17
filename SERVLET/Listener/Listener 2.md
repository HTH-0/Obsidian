# 🌐 서블릿 리스너 정리 - Session & Request 중심

---

## 📄 C03HttpSessionListener.java

```java
package Listener;

import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

@WebListener
public class C03HttpSessionListener implements HttpSessionListener{

	@Override
	public void sessionCreated(HttpSessionEvent se) {
		System.out.println("[LISTENER] C03HttpSessionListener Created...");
	}

	@Override
	public void sessionDestroyed(HttpSessionEvent se) {
		System.out.println("[LISTENER] C03HttpSessionListener Destroyed...");
	}
}
```

---

## 📄 C04HttpSessionListener.java

```java
package Listener;

import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSessionEvent;
import javax.servlet.http.HttpSessionListener;

@WebListener
public class C04HttpSessionListener implements HttpSessionListener{
	
	@Override
	public void sessionCreated(HttpSessionEvent se) {
		System.out.println("[LISTENER] C03 HTT~~ Lis ~~ created");
	}
	@Override
	public void sessionDestroyed(HttpSessionEvent se) {
		System.out.println("[LISTENER] C03 HTT~~ Lis ~~ destroyed");
	}
}
```

---

## 📄 C04HttpSessionAttributeListener.java

```java
package Listener;

import javax.servlet.annotation.WebListener;
import javax.servlet.http.HttpSessionAttributeListener;
import javax.servlet.http.HttpSessionBindingEvent;

@WebListener
public class C04HttpSessionAttributeListener implements HttpSessionAttributeListener{

	@Override
	public void attributeAdded(HttpSessionBindingEvent event) {

	}

	@Override
	public void attributeRemoved(HttpSessionBindingEvent event) {
	
	}

	@Override
	public void attributeReplaced(HttpSessionBindingEvent event) {
	
	}
}
```

---

## 📄 C05ServletRequestListener.java

```java
package Listener;

import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class C05ServletRequestListener implements ServletRequestListener{

	@Override
	public void requestDestroyed(ServletRequestEvent sre) {
		ServletRequestListener.super.requestDestroyed(sre);
	}

	@Override
	public void requestInitialized(ServletRequestEvent sre) {
		ServletRequestListener.super.requestInitialized(sre);
	}
}
```

---

## 📄 C06ServletRequestAttributeListener.java

```java
package Listener;

import javax.servlet.ServletRequestAttributeEvent;
import javax.servlet.ServletRequestAttributeListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class C06ServletRequestAttributeListener implements ServletRequestAttributeListener{

	@Override
	public void attributeAdded(ServletRequestAttributeEvent srae) {
		ServletRequestAttributeListener.super.attributeAdded(srae);
	}

	@Override
	public void attributeRemoved(ServletRequestAttributeEvent srae) {
		ServletRequestAttributeListener.super.attributeRemoved(srae);
	}

	@Override
	public void attributeReplaced(ServletRequestAttributeEvent srae) {
		ServletRequestAttributeListener.super.attributeReplaced(srae);
	}
}
```

---

## 🧠 코드 설명

### ✅ C03HttpSessionListener / C04HttpSessionListener

- 공통점: `HttpSessionListener` 구현
    
- `sessionCreated(HttpSessionEvent se)`  
    → 새로운 세션 생성 시 호출됨  
    → ex: 클라이언트가 처음 서버 접근 시
    
- `sessionDestroyed(HttpSessionEvent se)`  
    → 세션이 무효화될 때 호출됨  
    → ex: 로그아웃, 타임아웃
    
- 로그 메시지에 차이는 있으나 기능은 동일
    

---

### ✅ C04HttpSessionAttributeListener

- `HttpSessionAttributeListener` 구현
    
- 세션에 데이터 추가/삭제/변경 이벤트 감지
    
    - `attributeAdded()`  
        → `session.setAttribute("id", value);`
        
    - `attributeRemoved()`  
        → `session.removeAttribute("id");`
        
    - `attributeReplaced()`  
        → 동일 키로 값 변경 시 호출
        
- 현재 로그 없음 → 테스트용/미완성 상태
    

---

### ✅ C05ServletRequestListener

- `ServletRequestListener` 구현
    
- 클라이언트의 HTTP 요청 단위로 감지
    
    - `requestInitialized()`  
        → 클라이언트가 요청을 보낼 때 실행
        
    - `requestDestroyed()`  
        → 요청 처리가 끝나고 response 전송 직전
        
- `super.method()`만 존재 → 직접 정의된 로직 없음
    

---

### ✅ C06ServletRequestAttributeListener

- `ServletRequestAttributeListener` 구현
    
- 요청 범위 객체에 속성 추가/삭제/변경 이벤트 감지
    
    - `request.setAttribute(...)` 등 감지
        
- 메서드 내부에 구현 없음 (default 메서드 호출만 있음)
    

---

