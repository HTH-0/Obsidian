# 📂 Java 코드 분석: C00HelloWorld

이 문서는 **Java** 코드의 구조와 동작 원리를 설명하며, **Obsidian**에서 활용할 수 있도록 Markdown 형식과 이모지를 사용하여 가독성을 높였습니다.

---

## 🔹 1. 코드 개요

- **목적:**
    - 이 코드는 Java의 가장 기본적인 "Hello World" 프로그램입니다.
    - 콘솔에 **"HELLOWORLD"**라는 문자열을 출력합니다.
- **주요 특징:**
    - 패키지 선언(`package`)을 통해 코드의 구분을 명확히 함
    - **클래스**와 **메서드**의 기본 구조를 이해할 수 있음
    - `main` 메서드를 통해 프로그램의 실행 진입점을 보여줌

---

## 🔹 2. 코드 세부 분석

### 📁 패키지 선언

- **코드:** `package Ch00;`
- **설명:**
    - `Ch00` 패키지에 속한 코드임을 명시합니다.
    - 패키지는 관련 클래스들을 그룹화하여 관리하기 쉽게 도와줍니다.

---

### 🏷 클래스 선언

- **코드:** `public class C00HelloWorld { ... }`
- **설명:**
    - `C00HelloWorld`는 클래스의 이름입니다.
    - 클래스는 객체지향 프로그래밍(OOP)에서 코드의 기본 단위로, 관련 변수와 메서드를 하나의 묶음으로 관리합니다.

---

### 🔑 main 메서드

- **코드:** `public static void main(String[] args) { ... }`
- **설명:**
    - **메서드 종류:**
        - **라이브러리 메서드:** 미리 정의되어 제공되는 메서드
        - **사용자정의 메서드:** 개발자가 직접 구현하는 메서드
        - **main 메서드:** 프로그램 실행 시 가장 먼저 호출되는 메서드
    - `main` 메서드는 Java 프로그램의 시작점으로, 프로그램이 실행되면 제일 먼저 이 메서드가 호출됩니다.

---

### 💻 출력 구문

- **코드:** `System.out.println("HELLOWORLD");`
- **설명:**
    - `System.out.println`은 **라이브러리 메서드**로, 콘솔에 메시지를 출력합니다.
    - 이 코드는 "HELLOWORLD"를 출력하여 프로그램의 동작을 확인할 수 있게 합니다.

---

## 🔹 3. 전체 코드

```java
package Ch00;  // 패키지 구별 코드

public class C00HelloWorld {  // 클래스 영역 (객체지향 문법 적용 범위)
    public static void main(String[] args) {  // 메서드 영역 (절차지향 문법 적용 범위)
    
        // 메서드 종류
        // 라이브러리 메서드  : 미리 만들어져 제공되는 메서드
        // 사용자정의 메서드  : 개발자에 의해 만들어지는 메서드
        // main 메서드        : 최초 실행되는 메서드
        
        System.out.println("HELLOWORLD");
    }
}
```

---

## 🔹 4. 예상 실행 결과

- **콘솔 출력:**
    
    ```
    HELLOWORLD
    ```
    

---

## 🔹 5. 추가 학습 자료

- **Java 공식 튜토리얼:** [Oracle Java Tutorials](https://docs.oracle.com/javase/tutorial/)
- **Java 입문자 가이드:** [Java Programming Basics](https://www.w3schools.com/java/)
- **객체지향 프로그래밍(OOP) 이해:** [OOP Concepts](https://www.geeksforgeeks.org/object-oriented-programming-oops-concept-in-java/)

---

> **요약:**  
> 이 코드는 **패키지**, **클래스**, **main 메서드** 등의 기본 구성요소를 통해 "Hello World" 메시지를 콘솔에 출력하는 간단한 Java 프로그램입니다.  
> 🔹 **포인트:** 각 구성요소의 역할을 이해하며, 추후 더 복잡한 프로그램 개발 시 기반이 되는 개념들을 익힐 수 있습니다.
