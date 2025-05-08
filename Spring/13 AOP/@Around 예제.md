
---

# 🧭 Spring AOP – @Around 성능 측정 로깅 예제

## ✅ 목적

- `@Around` 어노테이션을 사용해 메서드 실행 전/후를 감싸고, 실제 **처리 시간(실행 시간)을 측정**
    
- 로그를 통해 어떤 메서드가 얼마나 오래 걸렸는지 확인 가능
    
- 실무에서는 **성능 모니터링**, **리소스 분석**, **보안 검사**, **트랜잭션 제어** 등에 응용됨
    

---

## 📦 전체 코드

### 1. LogTimerAspect.java (Aspect 클래스)

```java
package com.example.app.aspect;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LogTimerAspect {

    // com.example.service 패키지 하위 모든 클래스의 모든 메서드에 적용
    @Around("execution(* com.example.service.*.*(..))")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        // 시작 시간 기록
        long start = System.currentTimeMillis();

        // 실제 비즈니스 메서드 실행
        Object result = joinPoint.proceed();

        // 종료 시간 기록
        long end = System.currentTimeMillis();

        // 메서드 정보 및 실행 시간 출력
        String methodName = joinPoint.getSignature().toShortString();
        System.out.println(methodName + " 실행 시간: " + (end - start) + "ms");

        return result; // 원래 결과 리턴
    }
}
```

---

### 2. SampleService.java (비즈니스 클래스)

```java
package com.example.service;

import org.springframework.stereotype.Service;

@Service
public class SampleService {

    public void doSomething() throws InterruptedException {
        // 일부러 1초간 대기
        Thread.sleep(1000);
        System.out.println("비즈니스 로직 실행 중...");
    }
}
```

---

### 3. AopTest.java (JUnit 테스트 클래스)

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("file:src/main/webapp/WEB-INF/spring/root-context.xml")
public class AopTest {

    @Autowired
    private SampleService sampleService;

    @Test
    public void testAroundAdvice() throws Exception {
        sampleService.doSomething();
    }
}
```

---

### 4. root-context.xml 설정

```xml
<!-- AOP 활성화 -->
<aop:aspectj-autoproxy />

<!-- 컴포넌트 스캔 -->
<context:component-scan base-package="com.example.service, com.example.app.aspect"/>
```

---

## 🔍 코드 설명

### ✅ 핵심 어노테이션

- `@Aspect`: 해당 클래스가 AOP 관점(Aspect)임을 선언
    
- `@Around`: 타겟 메서드를 감싸기 위한 Advice. 실행 전/후, 예외까지 모두 제어 가능
    
- `execution(* com.example.service.*.*(..))`:
    
    - `com.example.service` 패키지 안의 **모든 클래스, 모든 메서드** 대상으로 지정
        

### ✅ ProceedingJoinPoint

- `proceed()`: 실제 타겟 메서드 실행을 위임
    
- 메서드 실행 전후로 커스텀 로직 삽입 가능 (타이머, 권한 검사 등)
    
- `getSignature().toShortString()`: 어떤 메서드가 호출되었는지 짧은 설명 반환
    

---

## 📌 실행 결과 예시

```txt
비즈니스 로직 실행 중...
SampleService.doSomething() 실행 시간: 1004ms
```

---

## 📌 요약

- `@Around` Advice는 타겟 메서드를 감싸서 전후 로직을 모두 제어 가능
    
- 성능 측정(처리 시간 로그) 외에도,
    
    - 트랜잭션 시작/종료
        
    - 리소스 사용량 확인
        
    - 권한 체크
        
    - 예외 처리 등 다양한 응용 가능
        
- `@Around` + `ProceedingJoinPoint` 조합은 Spring AOP의 가장 강력한 기능 중 하나
    

---
