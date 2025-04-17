
---

## 📄 C01ServletContextListener.java

```java
package Listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

//@WebListener
public class C01ServletContextListener implements ServletContextListener{

	@Override
	public void contextInitialized(ServletContextEvent sce) {
		//----
		System.out.println("[LISTENER] C01ServletContextListener..start...");
	}
	
	@Override
	public void contextDestroyed(ServletContextEvent sce) {
		System.out.println("[LISTENER] C01ServletContextListener..end...");
	}
}
```

---

## 📄 C02ServletContextAttributeListener.java

```java
package Listener;

import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class C02ServletContextAttributeListener implements ServletContextAttributeListener{

	@Override
	public void attributeAdded(ServletContextAttributeEvent scae) {
		System.out.println("[LISTENER] C02ServletContextAttributeListener add()..");
	}

	@Override
	public void attributeRemoved(ServletContextAttributeEvent scae) {
		System.out.println("[LISTENER] C02ServletContextAttributeListener remove()..");
	}

	@Override
	public void attributeReplaced(ServletContextAttributeEvent scae) {
		System.out.println("[LISTENER] C02ServletContextAttributeListener replace()..");
	}
}
```

---

## 📄 C02ServletContextListener.java

```java
package Listener;

import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;

public class C02ServletContextListener implements ServletContextAttributeListener{

	@Override
	public void attributeAdded(ServletContextAttributeEvent event) {
		System.out.println("[LISTENER] ");
	}

	@Override
	public void attributeRemoved(ServletContextAttributeEvent event) {
		System.out.println("[LISTENER] ");
	}

	@Override
	public void attributeReplaced(ServletContextAttributeEvent event) {
		System.out.println("[LISTENER] ");
	}
}
```

---

## 🧠 코드 설명

### ✅ C01ServletContextListener

- `ServletContextListener` 구현체
    
- `contextInitialized()` : 서버 시작 시 호출됨
    
    - 콘솔에 "[LISTENER] C01ServletContextListener..start..." 출력
        
- `contextDestroyed()` : 서버 종료 시 호출됨
    
    - 콘솔에 "[LISTENER] C01ServletContextListener..end..." 출력
        
- 주로 애플리케이션 전체 생명주기에서 실행할 초기화 또는 종료 작업 수행할 때 사용
    

---

### ✅ C02ServletContextAttributeListener

- `ServletContextAttributeListener` 구현체
    
- `@WebListener` 붙어서 자동 등록됨
    
- `attributeAdded()` : `ServletContext`에 속성이 추가될 때 호출됨
    
    - 예: `application.setAttribute("key", value);`
        
- `attributeRemoved()` : `ServletContext`에서 속성 제거 시 호출
    
- `attributeReplaced()` : 동일 key에 대해 값이 변경될 때 호출됨
    
- 각 메서드에서 콘솔 로그 출력해 변경 감지 확인 가능
    

---

### ✅ C02ServletContextListener (미완성 예제)

- 동일하게 `ServletContextAttributeListener` 구현
    
- 하지만 로그 출력 메시지가 비어 있음 → 실제 프로젝트에서는 미완성이거나 테스트용일 가능성 있음
    
- `C02ServletContextAttributeListener`와 기능적으로 동일한 구조이나, 로그 구분이 어려움
    

---
