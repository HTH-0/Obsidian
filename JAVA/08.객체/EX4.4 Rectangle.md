---
sticker: emoji//1f4ac
---
### 📌 Java 클래스 및 사각형 포함 관계 - `Rectangle` 클래스 예제

---

## 📌 개요

- `Rectangle` 클래스를 정의하여 사각형 정보를 저장
- 사각형의 면적을 계산하는 `square()` 메서드 구현
- 사각형이 다른 사각형을 포함하는지 판별하는 `contains()` 메서드 구현

---

## 💻 코드 정리

```java
package Ch08;

class Rectangle {
    int x, y; // 좌표
    int width, height; // 가로, 세로 크기

    // 생성자
    Rectangle(int x, int y, int width, int height) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
    }

    // 면적 계산 메서드
    int square() {
        return width * height;
    }

    // 사각형 정보 출력 메서드
    void show() {
        System.out.printf("(%d,%d)에서 크기가 %dx%d인 사각형\n", x, y, width, height);
    }

    // 사각형 포함 여부 확인 메서드
    boolean contains(Rectangle r) {
        if (x < r.x && y < r.y && (x + width) > (r.x + r.width) && (y + height) > (r.y + r.height)) {
            return true;
        }
        return false;
    }
}

public class Ex4_4Rectangle {
    public static void main(String[] args) {
        Rectangle r = new Rectangle(2, 2, 8, 7);
        Rectangle s = new Rectangle(5, 5, 6, 6);
        Rectangle t = new Rectangle(1, 1, 10, 10);

        r.show();
        System.out.println("s의 면적은 " + s.square());

        if (t.contains(r))
            System.out.println("t는 r을 포함합니다.");
        if (t.contains(s))
            System.out.println("t는 s를 포함합니다.");
    }
}
```

---

## 🔎 코드 분석

### 1️⃣ **클래스 정의 및 멤버 변수**

```java
class Rectangle {
    int x, y;
    int width, height;
```

- `x, y`: 사각형의 좌표 (좌측 상단)
- `width, height`: 사각형의 가로, 세로 크기

### 2️⃣ **생성자 정의**

```java
Rectangle(int x, int y, int width, int height) {
    this.x = x;
    this.y = y;
    this.width = width;
    this.height = height;
}
```

- `this` 키워드를 사용하여 인스턴스 변수와 매개변수를 구분

### 3️⃣ **면적 계산 메서드 (`square()`)**

```java
int square() {
    return width * height;
}
```

- `width * height`로 사각형의 면적을 반환

### 4️⃣ **사각형 정보 출력 메서드 (`show()`)**

```java
void show() {
    System.out.printf("(%d,%d)에서 크기가 %dx%d인 사각형\n", x, y, width, height);
}
```

- 사각형의 좌표와 크기를 출력

### 5️⃣ **포함 관계 판별 메서드 (`contains()`)**

```java
boolean contains(Rectangle r) {
    if (x < r.x && y < r.y && (x + width) > (r.x + r.width) && (y + height) > (r.y + r.height)) {
        return true;
    }
    return false;
}
```

- 현재 사각형(`this`)이 다른 사각형 `r`을 포함하는 조건
    - `x < r.x && y < r.y` → `this`의 좌측 상단이 `r`보다 더 위쪽, 왼쪽에 있어야 함
    - `(x + width) > (r.x + r.width)` → `this`의 우측 끝이 `r`보다 더 오른쪽에 있어야 함
    - `(y + height) > (r.y + r.height)` → `this`의 하단 끝이 `r`보다 더 아래에 있어야 함

### 6️⃣ **객체 생성 및 출력**

```java
Rectangle r = new Rectangle(2, 2, 8, 7);
Rectangle s = new Rectangle(5, 5, 6, 6);
Rectangle t = new Rectangle(1, 1, 10, 10);
```

- `r(2,2,8,7)`, `s(5,5,6,6)`, `t(1,1,10,10)` 사각형 생성

```java
r.show();
System.out.println("s의 면적은 " + s.square());
```

- `r`의 정보 출력
- `s`의 면적 출력

```java
if (t.contains(r))
    System.out.println("t는 r을 포함합니다.");
if (t.contains(s))
    System.out.println("t는 s를 포함합니다.");
```

- `t`가 `r`을 포함하는지 검사 → 포함하면 메시지 출력
- `t`가 `s`를 포함하는지 검사 → 포함하면 메시지 출력

---

## 📌 실행 결과

```plaintext
(2,2)에서 크기가 8x7인 사각형
s의 면적은 36
t는 r을 포함합니다.
```

- `t(1,1,10,10)`이 `r(2,2,8,7)`을 포함하므로 메시지 출력
- `t`는 `s(5,5,6,6)`을 포함하지 않으므로 해당 메시지는 출력되지 않음

---

## 🔎 추가 정보

- **사각형 포함 여부 판단 방법**
    - 사각형 A가 사각형 B를 포함하려면 B의 네 꼭짓점이 모두 A 내부에 있어야 함
    - 이를 수식으로 나타내면:
        
        ```
        A.x < B.x && A.y < B.y &&
        A.x + A.width > B.x + B.width &&
        A.y + A.height > B.y + B.height
        ```
        
- **사각형이 겹치는 경우를 판별하려면?**
    - 현재 코드에서는 완전히 포함 여부만 확인 가능
    - 두 사각형이 겹치는지 여부를 판별하려면 추가적인 조건 필요

---

## 📌 관련 문서

```dataviewjs
async function loadFiles() {
    let pages = dv.pages('"JAVA"');  

    let results = [];

    for (let p of pages) {
        let content = await dv.io.load(p.file.path); 
        if (content.includes("사각형") || content.includes("contains()") || content.includes("면적 계산")) {
            results.push([p.file.link]); 
        }
    }

    dv.table(["파일 이름"], results);
}

loadFiles();
```