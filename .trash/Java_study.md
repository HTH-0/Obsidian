# 📝 Java 기본 구조 - `Hello World`

## 📌 내용

- **Java 기본 구조 학습**
- 패키지 선언, 클래스 선언, 메서드 정의
- `main()` 메서드의 역할
- `System.out.println()`을 이용한 출력

---

## 🔍 코드

```java
package Ch00;  // [[패키지 선언 (Java)]]

public class C00HelloWorld {  // [[클래스 선언 (Java)]]
    public static void main(String[] args) {  // [[main 메서드 (Java)]]
    
        // 메서드 종류
        
        // 라이브러리 메서드 : 미리 만들어져 제공되는 메서드
        // 사용자정의 메서드 : 개발자에 의해 만들어지는 메서드
        // main 메서드 : 최초 실행되는 메서드
        
        System.out.println("HELLOWORLD");  // [[System.out.println() (Java)]]
    }
}
```

---

## 🔎 코드 분석

### 1️⃣ **패키지 선언 (`package`)**

```java
package Ch00;
```

- **패키지**: 관련된 클래스들을 묶어 관리하는 논리적 단위
- `Ch00`이라는 패키지 안에 `C00HelloWorld` 클래스가 포함됨

### 2️⃣ **클래스 선언 (`class`)**

```java
public class C00HelloWorld {
```

- Java 프로그램의 최소 단위는 **클래스**
- `C00HelloWorld`라는 이름의 클래스를 선언

### 3️⃣ **메인 메서드 (`main()`)**

```java
public static void main(String[] args) {
```

- **프로그램 실행의 시작점**
- `public` → 어디서든 접근 가능
- `static` → 인스턴스 생성 없이 실행 가능
- `void` → 반환값 없음
- `String[] args` → 명령줄 인자를 받을 수 있음

### 4️⃣ **문자열 출력 (`System.out.println()`)**

```java
System.out.println("HELLOWORLD");
```

- `System.out.println()` → 콘솔에 문자열 출력
- `"HELLOWORLD"`가 화면에 출력됨

---

## 🔎 추가 정보

- **라이브러리 메서드**: Java에서 제공하는 내장 메서드 (`System.out.println()`, `Math.sqrt()` 등)
- **사용자 정의 메서드**: 개발자가 직접 작성한 메서드
- **main() 메서드**: Java 프로그램의 진입점으로 반드시 필요

---

## 📌 관련 문서 (내용 기반 검색)

```dataviewjs
const pages = dv.pages()
    .filter(p => p.file.path.includes("JAVA") && p.file.name !== dv.current().file.name && typeof p.content === "string")
    .filter(p => {
        let content = p.content ? p.content.toLowerCase() : ""; // undefined 방지
        return ["패키지", "클래스", "메서드", "System.out", "main"].some(k => new RegExp(k, "i").test(content));
    });

dv.header(2, "📌 관련 문서");
pages.length ? dv.list(pages.map(p => `[[${p.file.name}]]`)) : dv.paragraph("🔍 관련 문서 없음");

```