---
sticker: emoji//1f4ac
---
### 📌 Java 클래스 및 객체 생성 - `TV` 클래스 예제

---

## 📌 개요

- `TV` 클래스를 정의하고 생성자를 사용하여 객체를 생성
- 객체의 정보를 출력하는 `show()` 메서드 구현

---

## 💻 코드 정리

```java
package Ch08;

class TV {
    String company;
    int year;
    int size;

    // 생성자
    TV(String company, int year, int size) {
        this.company = company;
        this.year = year;
        this.size = size;
    }

    // TV 정보 출력 메서드
    void show() {
        System.out.printf("%s에서 만든 %d년 %d인치 TV\n", company, year, size);
    }
}

public class Ex4_1Tv {
    public static void main(String[] args) {
        TV myTV = new TV("LG", 2017, 32); // TV 객체 생성
        myTV.show(); // TV 정보 출력
    }
}
```

---

## 🔎 코드 분석

### 1️⃣ **클래스 정의**

```java
class TV {
    String company;
    int year;
    int size;
```

- `company`: TV 제조사 (`String` 타입)
- `year`: TV 출시 연도 (`int` 타입)
- `size`: TV 크기 (`int` 타입)

### 2️⃣ **생성자 정의**

```java
TV(String company, int year, int size) {
    this.company = company;
    this.year = year;
    this.size = size;
}
```

- 생성자는 객체가 생성될 때 자동으로 실행됨
- `this` 키워드를 사용하여 멤버 변수와 매개변수를 구분

### 3️⃣ **TV 정보 출력 메서드 (`show()`)**

```java
void show() {
    System.out.printf("%s에서 만든 %d년 %d인치 TV\n", company, year, size);
}
```

- `printf()`를 사용하여 TV 정보를 출력

### 4️⃣ **객체 생성 및 메서드 호출**

```java
TV myTV = new TV("LG", 2017, 32);
myTV.show();
```

- `TV` 객체를 생성하고 `show()` 메서드 호출

---

## 📌 실행 결과

```plaintext
LG에서 만든 2017년 32인치 TV
```

---

## 🔎 추가 정보

- **클래스 내 `this` 키워드**
    - 인스턴스 변수와 생성자의 매개변수 이름이 같을 때 구분하기 위해 사용
- **`printf()`와 `println()` 차이점**
    - `printf()` → 형식 지정자를 사용하여 출력
    - `println()` → 개행 포함 출력

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