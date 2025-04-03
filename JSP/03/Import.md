---

# 📄 JSP에서 Java 객체 사용하기 (DTO 출력 예제)

## 📌 개념 요약

JSP에서 Java 객체(DTO)를 생성하고 데이터를 `<%= %>` 구문을 통해 HTML에 출력하는 방법.

---

## ✅ 주요 내용

- `<%@ page import="..." %>` 지시어를 통해 Java 클래스 가져오기
    
- JSP 스크립틀릿 `<% %>` 내부에서 Java 객체 생성 및 초기화 가능
    
- 생성한 객체의 데이터를 HTML 영역에 출력할 때는 `<%= %>` 사용
    
- DTO (Data Transfer Object)는 데이터를 전달하거나 표현할 때 사용하는 단순 클래스
    

---

## 💻 코드 예시

### 📎 SimpleDTO.java (DTO 클래스)

```java
package C03;

public class SimpleDTO {
    private String name;
    private int age;
    private String addr;

    // 생성자
    public SimpleDTO(String name, int age, String addr) {
        this.name = name;
        this.age = age;
        this.addr = addr;
    }

    // Getter 메서드
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getAddr() {
        return addr;
    }

    @Override
    public String toString() {
        return name + ", " + age + ", " + addr;
    }
}
```

### 📎 sample.jsp (JSP 페이지)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="C03.SimpleDTO" %>    

<%
    // DTO 객체 생성
    SimpleDTO dto = new SimpleDTO("홍길동", 55, "대구");

    // 콘솔에 출력 (서버 콘솔에 보임)
    System.out.println("DTO : " + dto);
%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>DTO 출력 예제</title>
</head>
<body>
    name : <%= dto.getName() %> <br/>
    age : <%= dto.getAge() %> <br/>
    addr : <%= dto.getAddr() %> <br/>
</body>
</html>
```

---

## 🧾 실행 결과 예시 (웹 브라우저 출력)

```
name : 홍길동
age : 55
addr : 대구
```

---

## 📌 정리

- JSP에서는 자바 객체를 생성해 데이터를 담고 `<%= %>`로 출력 가능
    
- DTO 클래스는 데이터를 담는 용도만 담당
    
- `System.out.println()`은 브라우저가 아닌 _서버 콘솔에만_ 출력됨
    

---

## 🔎 추가 개념

- `Scanner` 객체는 웹 환경(JSP)에서 사용 불가 → `System.in`은 터미널 입력 전용이라 브라우저 입력과 무관  
    → HTML 폼을 통한 사용자 입력은 `<form>` + `request.getParameter()` 방식 사용
    
- JSP에서 로직 처리보다는 가급적 Java 클래스(Service/Controller 등)에 위임하는 구조가 바람직 (MVC 패턴)
    

---
