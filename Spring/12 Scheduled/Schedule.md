
---

# 🕒 Spring Scheduling 실습 정리 (STS3 + Spring 5.0.7.RELEASE)

## 📦 전체 코드

### ✅ 스케줄 설정 클래스 - `SchedulingConfig.java`

```java
@Configuration
@EnableScheduling
public class SchedulingConfig implements SchedulingConfigurer {

    @Override
    public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {
        ThreadPoolTaskScheduler taskScheduler = new ThreadPoolTaskScheduler();
        taskScheduler.setPoolSize(5); // 5개 쓰레드 병렬 실행
        taskScheduler.setThreadNamePrefix("MyScheduler-");
        taskScheduler.initialize();
        taskRegistrar.setTaskScheduler(taskScheduler);
    }
}
```

---

### ✅ 스케줄러 컴포넌트 - `MyScheduler.java`

```java
@Component
public class MyScheduler {

    @Scheduled(fixedRate = 5000)
    public void fixedRateJob() {
        System.out.println("fixedRate 실행 : " + new Date());
    }

    @Scheduled(fixedDelay = 3000)
    public void fixedDelayJob() {
        System.out.println("fixedDelay 실행 : " + new Date());
    }

    @Scheduled(initialDelay = 1000, fixedRate = 2000)
    public void initialDelayJob() {
        System.out.println("initialDelay + fixedRate 실행 : " + new Date());
    }

    @Scheduled(cron = "0/10 * * * * *")
    public void cronJob() {
        System.out.println("cron 표현식 실행 : " + new Date());
    }
}
```

---

## 🔍 핵심 개념 및 설정

### ✅ 스케줄링 기본 개념

- `@Scheduled`: 주기적인 작업을 자동으로 실행하도록 설정하는 어노테이션
    
- `@EnableScheduling`: 스케줄 기능 활성화를 위한 어노테이션 (자바 설정 파일 또는 XML에서 사용)
    

---

### ✅ 주요 속성 정리

|속성명|설명|
|---|---|
|`fixedRate`|이전 작업 시작 후 지정 시간(ms)마다 실행|
|`fixedDelay`|이전 작업 종료 후 지정 시간(ms)마다 실행|
|`initialDelay`|최초 시작까지 대기 시간 (ms)|
|`cron`|cron 표현식 기반으로 실행 주기 지정|

---

### ✅ cron 표현식 예시

|표현식|설명|
|---|---|
|`0/5 * * * * *`|매 5초마다 실행|
|`0 0/1 * * * *`|매 분마다 실행 (0초 기준)|
|`0 0 9 * * *`|매일 오전 9시에 실행|

---

### ✅ 병렬 실행을 위한 ThreadPool 설정

- 기본 설정은 단일 쓰레드 → 하나의 작업만 순차적으로 실행됨
    
- 병렬 작업이 필요할 경우 `ThreadPoolTaskScheduler`를 수동 등록
    
- `SchedulingConfigurer`를 구현하고 `ScheduledTaskRegistrar`에 커스텀 스케줄러 설정
    

---

## 📌 요약

- `@EnableScheduling` + `@Scheduled` 조합으로 스케줄링 기능 활성화
    
- 다양한 실행 방식 제공: `fixedRate`, `fixedDelay`, `initialDelay`, `cron`
    
- 정확한 시간 제어가 필요할 경우 `cron` 표현식 사용
    
- 복수 작업 병렬 실행을 위해 `ThreadPoolTaskScheduler` 설정 가능 (`SchedulingConfigurer` 구현)
    

---
