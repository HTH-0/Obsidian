---
sticker: emoji//1f4ac
---
### 📌 Java 클래스 및 평균 계산 - `Grade` 클래스 예제

---

## 📌 개요

- `Grade` 클래스를 정의하여 과목 점수를 저장하고 평균을 계산
- `Scanner`를 사용하여 사용자 입력을 받아 객체 생성
- `average()` 메서드를 통해 평균 점수 계산

---

## 💻 코드 정리

```java
package Ch08;
import java.util.Scanner;

class Grade {
    int subject1;
    int subject2;
    int subject3;

    // 생성자
    Grade(int subject1, int subject2, int subject3) {
        this.subject1 = subject1;
        this.subject2 = subject2;
        this.subject3 = subject3;
    }

    // 평균 계산 메서드
    public double average() {
        return (double) (subject1 + subject2 + subject3) / 3;
    }
}

public class Ex4_2Grade {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("수학, 과학, 영어 순으로 3개의 정수 입력 >> ");
        int math = sc.nextInt();
        int science = sc.nextInt();
        int english = sc.nextInt();

        // Grade 객체 생성
        Grade me = new Grade(math, science, english);
        
        // 평균 출력
        System.out.println("평균은 " + me.average()); 
        
        sc.close();
    }
}
```

---

## 🔎 코드 분석

### 1️⃣ **클래스 정의 및 멤버 변수**

```java
class Grade {
    int subject1;
    int subject2;
    int subject3;
```

- `subject1`, `subject2`, `subject3`: 각 과목의 점수를 저장하는 변수

### 2️⃣ **생성자 정의**

```java
Grade(int subject1, int subject2, int subject3) {
    this.subject1 = subject1;
    this.subject2 = subject2;
    this.subject3 = subject3;
}
```

- 생성자를 통해 세 개의 과목 점수를 초기화
- `this` 키워드를 사용하여 인스턴스 변수와 매개변수를 구분

### 3️⃣ **평균 계산 메서드 (`average()`)**

```java
public double average() {
    return (double) (subject1 + subject2 + subject3) / 3;
}
```

- `(double)`을 이용하여 형 변환 → 정수 연산으로 인한 소수점 손실 방지
- 과목 점수의 합을 `3`으로 나누어 평균을 계산하여 반환

### 4️⃣ **사용자 입력 받기 (`Scanner`)**

```java
Scanner sc = new Scanner(System.in);
System.out.print("수학, 과학, 영어 순으로 3개의 정수 입력 >> ");
int math = sc.nextInt();
int science = sc.nextInt();
int english = sc.nextInt();
```

- `Scanner`를 사용하여 정수 값 입력 받기

### 5️⃣ **객체 생성 및 평균 출력**

```java
Grade me = new Grade(math, science, english);
System.out.println("평균은 " + me.average());
```

- 입력받은 점수를 이용하여 `Grade` 객체 생성
- `average()` 메서드를 호출하여 평균 출력

### 6️⃣ **자원 해제 (`close()`)**

```java
sc.close();
```

- `Scanner` 객체를 닫아 리소스 해제

---

## 📌 실행 예시

```plaintext
수학, 과학, 영어 순으로 3개의 정수 입력 >> 80 90 85
평균은 85.0
```

---

## 🔎 추가 정보

- **`Scanner`를 이용한 입력 처리**
    - `nextInt()` → 정수 입력
    - 입력 후 `close()` 호출하여 메모리 누수 방지
- **`(double)`을 이용한 형 변환**
    - `(subject1 + subject2 + subject3) / 3` → 정수 나눗셈으로 인해 소수점 손실
    - `(double)(subject1 + subject2 + subject3) / 3` → 실수 연산 수행

---

## 📌 관련 문서

```dataviewjs
async function loadFiles() {
    let pages = dv.pages('"JAVA"');  

    let results = [];

    for (let p of pages) {
        let content = await dv.io.load(p.file.path); 
        if (content.includes("Scanner") || content.includes("nextInt()") || content.includes("평균 계산")) {
            results.push([p.file.link]); 
        }
    }

    dv.table(["파일 이름"], results);
}

loadFiles();
```