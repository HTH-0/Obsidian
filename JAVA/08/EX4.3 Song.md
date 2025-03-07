---
sticker: emoji//1f4ac
---
### 📌 Java 클래스 및 객체 생성 - `Song` 클래스 예제

---

## 📌 개요

- `Song` 클래스를 정의하여 노래 정보를 저장
- 생성자를 사용하여 객체를 생성하고 `show()` 메서드로 출력

---

## 💻 코드 정리

```java
package Ch08;

class Song {
    String title;
    String artist;
    int year;
    String country;

    // 생성자
    Song(String title, String artist, int year, String country) {
        this.title = title;
        this.artist = artist;
        this.year = year;
        this.country = country;
    }

    // 노래 정보 출력 메서드
    void show() {
        System.out.printf("%d년 %s 국적의 %s가 부른 %s\n", year, country, artist, title);
    }
}

public class Ex4_3Song {
    public static void main(String[] args) {
        // Song 객체 생성
        Song song = new Song("Dancing Queen", "ABBA", 1978, "스웨덴");

        // 노래 정보 출력
        song.show();
    }
}
```

---

## 🔎 코드 분석

### 1️⃣ **클래스 정의 및 멤버 변수**

```java
class Song {
    String title;
    String artist;
    int year;
    String country;
```

- `title`: 노래 제목 (`String` 타입)
- `artist`: 가수 이름 (`String` 타입)
- `year`: 발표 연도 (`int` 타입)
- `country`: 가수의 국적 (`String` 타입)

### 2️⃣ **생성자 정의**

```java
Song(String title, String artist, int year, String country) {
    this.title = title;
    this.artist = artist;
    this.year = year;
    this.country = country;
}
```

- 생성자를 통해 노래 정보를 초기화
- `this` 키워드를 사용하여 인스턴스 변수와 매개변수를 구분

### 3️⃣ **노래 정보 출력 메서드 (`show()`)**

```java
void show() {
    System.out.printf("%d년 %s 국적의 %s가 부른 %s\n", year, country, artist, title);
}
```

- `printf()`를 사용하여 형식에 맞게 노래 정보 출력

### 4️⃣ **객체 생성 및 출력**

```java
Song song = new Song("Dancing Queen", "ABBA", 1978, "스웨덴");
song.show();
```

- `Song` 객체를 생성하여 정보를 저장
- `show()` 메서드를 호출하여 노래 정보 출력

---

## 📌 실행 결과

```plaintext
1978년 스웨덴 국적의 ABBA가 부른 Dancing Queen
```

---

## 🔎 추가 정보

- **`this` 키워드 사용 이유**
    - 생성자의 매개변수와 클래스의 인스턴스 변수를 구분하기 위해 사용
- **`printf()`의 활용**
    - `%d` → 정수 출력
    - `%s` → 문자열 출력

---

## 📌 관련 문서

```dataviewjs
async function loadFiles() {
    let pages = dv.pages('"JAVA"');  

    let results = [];

    for (let p of pages) {
        let content = await dv.io.load(p.file.path); 
        if (content.includes("생성자") || content.includes("객체 생성") || content.includes("this")) {
            results.push([p.file.link]); 
        }
    }

    dv.table(["파일 이름"], results);
}

loadFiles();
```