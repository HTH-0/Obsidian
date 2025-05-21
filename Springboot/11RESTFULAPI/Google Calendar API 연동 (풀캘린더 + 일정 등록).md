---
aliases:
  - Google Calendar API 연동 (풀캘린더 + 일정 등록)**
---
---

# 📅 Spring Boot RESTFUL API 실습 정리 - Part 7

## 🟥 Part 7: Google Calendar API 연동 (FullCalendar + 일정 등록)

---

## ✅ 주요 개념

|항목|설명|
|---|---|
|프론트엔드|FullCalendar.js|
|백엔드|Spring Boot + Google Calendar API|
|기능|일정 조회, 클릭 시 상세 보기, 날짜 클릭 시 일정 추가|
|인증 방식|OAuth2 + client_secret.json|

---

## 📘 1. 일정 조회 (GET)

### ✅ HTML (`google/cal.html`)

```html
<div id="calendar"></div>

<script src='https://cdn.jsdelivr.net/npm/fullcalendar@6.1.17/index.global.min.js'></script>
<script src="https://cdn.jsdelivr.net/npm/@fullcalendar/google-calendar@6.1.17/index.global.min.js"></script>

<script>
  document.addEventListener('DOMContentLoaded', function () {
    var calendar = new FullCalendar.Calendar(document.getElementById('calendar'), {
      initialView: 'dayGridMonth',
      googleCalendarApiKey: 'YOUR_API_KEY',
      events: {
        googleCalendarId: 'YOUR_CALENDAR_ID@group.calendar.google.com'
      },
      eventClick: function(info) {
        info.jsEvent.preventDefault();
        console.log(info.event.title);
      }
    });
    calendar.render();
  });
</script>
```

---

## 📘 2. 일정 추가 (POST)

### ✅ HTML: 모달 폼

```html
<!-- modal 안에 폼 -->
<form action="/google/cal/post" name="dateForm">
  <input type="text" name="date" />
  <input type="text" name="title" />
  <textarea name="desc"></textarea>
  <button>일정 추가</button>
</form>
```

### ✅ 날짜 클릭 이벤트로 모달 활성화

```javascript
dateClick: function(info) {
  const form = document.dateForm;
  form.date.value = info.dateStr;
  document.querySelector(".date-event-modal").click(); // 모달 열기
}
```

---

## 📦 컨트롤러: `C02GoogleCalendarController`

```java
@Controller
@RequestMapping("/google")
@Slf4j
public class C02GoogleCalendarController {

    @GetMapping("/cal")
    public String main() {
        return "google/cal";
    }

    @GetMapping("/cal/post")
    public RedirectView post(@RequestParam String title,
                              @RequestParam String desc,
                              @RequestParam String dateStr) {
        try {
            SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
            Date selectedDate = sdf.parse(dateStr);

            Event event = new Event()
                    .setSummary(title)
                    .setDescription(desc);

            DateTime startDateTime = new DateTime(selectedDate);
            EventDateTime start = new EventDateTime()
                    .setDateTime(startDateTime)
                    .setTimeZone("Asia/Seoul");
            event.setStart(start);

            DateTime endDateTime = new DateTime(new Date(selectedDate.getTime() + 3600000));
            EventDateTime end = new EventDateTime()
                    .setDateTime(endDateTime)
                    .setTimeZone("Asia/Seoul");
            event.setEnd(end);

            GoogleCalendar.addEvent(event);

        } catch (Exception e) {
            log.error("Error adding event", e);
        }

        return new RedirectView("/google/cal");
    }
}
```

---

## 🛠 GoogleCalendar.java

```java
public class GoogleCalendar {
    private static final String APPLICATION_NAME = "Google Calendar API Java Quickstart";
    private static final String CALENDAR_ID = "your_calendar_id";
    private static final JsonFactory JSON_FACTORY = GsonFactory.getDefaultInstance();
    private static final String CREDENTIALS_FOLDER = "credentials";
    private static final String CLIENT_SECRET_DIR = "/client_secret.json";

    public static Event addEvent(Event event) throws Exception {
        NetHttpTransport transport = GoogleNetHttpTransport.newTrustedTransport();
        Calendar service = new Calendar.Builder(transport, JSON_FACTORY, getCredentials(transport))
                .setApplicationName(APPLICATION_NAME)
                .build();
        return service.events().insert(CALENDAR_ID, event).execute();
    }

    private static Credential getCredentials(NetHttpTransport transport) throws Exception {
        InputStream in = GoogleCalendar.class.getResourceAsStream(CLIENT_SECRET_DIR);
        GoogleClientSecrets secrets = GoogleClientSecrets.load(JSON_FACTORY, new InputStreamReader(in));

        GoogleAuthorizationCodeFlow flow = new GoogleAuthorizationCodeFlow.Builder(
                transport, JSON_FACTORY, secrets, List.of(CalendarScopes.CALENDAR))
                .setDataStoreFactory(new FileDataStoreFactory(new File(CREDENTIALS_FOLDER)))
                .setAccessType("offline")
                .build();

        return new AuthorizationCodeInstalledApp(flow, new LocalServerReceiver.Builder()
                .setPort(8888)
                .setCallbackPath("/google/cal/callback").build()).authorize("user");
    }
}
```

---

## ✅ 정리 요약

|항목|설명|
|---|---|
|일정 조회|FullCalendar + 구글 API 키로 public calendar 조회|
|일정 등록|모달에서 날짜/내용 → GoogleCalendar API POST|
|인증 방식|client_secret.json + 사용자 인증 (최초 1회 필요)|
|사용 클래스|`Event`, `EventDateTime`, `Credential`, `Calendar.Builder` 등|

---
