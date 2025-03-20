# **📌 람다식(Lambda Expression)과 인터페이스(Interface)

람다식(`Lambda Expression`)은 **Java 8부터 도입된 기능**으로, **함수를 간결하게 표현할 수 있는 방법**입니다.  
특히 **인터페이스(Interface)와 함께 사용**하면 코드가 훨씬 짧아지고 가독성이 좋아집니다.

---

# **🚀 1️⃣ 인터페이스(Interface)란?**

**인터페이스는 메서드의 "틀"만 정의하고, 실제 구현은 하지 않는 추상적인 클래스**입니다.  
즉, **추상 메서드(구현이 없는 메서드)만 포함**할 수 있으며, **클래스를 설계할 때 표준을 정하는 역할**을 합니다.

### **✅ 기본적인 인터페이스 선언**

```java
interface Printer {
    void print(String message); // 추상 메서드 (구현 없음)
}
```

✔ **인터페이스에는 `abstract` 키워드를 생략해도 자동으로 `abstract`(추상 메서드)로 인식됩니다.**

---

## **📌 인터페이스를 활용하는 방법**

### **✅ 1️⃣ 일반적인 구현 방식 (인터페이스를 클래스로 구현)**

```java
interface Printer {
    void print(String message);
}

// ✅ 인터페이스 구현
class ConsolePrinter implements Printer {
    @Override
    public void print(String message) {
        System.out.println("출력: " + message);
    }
}

public class Main {
    public static void main(String[] args) {
        Printer printer = new ConsolePrinter();
        printer.print("Hello, Interface!");
    }
}
```

### **🔹 실행 결과**

```plaintext
출력: Hello, Interface!
```

✔ **인터페이스를 구현한 클래스를 만들어야 해서 코드가 길어짐**  
✔ **이러한 불편함을 해결하기 위해 람다식(`Lambda`)이 등장!** 🚀

---

# **🚀 2️⃣ 람다식(Lambda Expression)**

## **📌 람다식이란?**

**람다식(`Lambda Expression`)은 함수형 프로그래밍을 지원하는 문법**으로,  
**불필요한 코드 없이 간결하게 함수를 표현하는 방법**입니다.  
Java에서는 **인터페이스의 "추상 메서드가 1개만 있을 때" 람다식을 사용할 수 있음** (함수형 인터페이스).

---

## **📌 람다식의 기본 문법**

```java
(매개변수) -> { 실행문 }
```

### **✅ 1️⃣ 기본적인 람다식 예제**

```java
Printer printer = (message) -> { System.out.println("출력: " + message); };
printer.print("Hello, Lambda!");
```

✔ **`Printer` 인터페이스를 구현하는 클래스를 만들 필요 없이 람다식 한 줄로 해결!**

---

# **🚀 3️⃣ 람다식과 인터페이스의 관계**

람다식은 **인터페이스의 메서드를 구현하는 익명 함수(Anonymous Function) 형태**입니다.  
즉, **람다식을 사용하려면 반드시 "추상 메서드가 하나만 존재하는 인터페이스"가 필요**합니다.

---

## **📌 함수형 인터페이스(Functional Interface)**

**람다식을 사용하려면 "추상 메서드가 하나만 있는 인터페이스"가 필요**합니다.  
이러한 인터페이스를 **"함수형 인터페이스(Functional Interface)"**라고 합니다.

```java
@FunctionalInterface // ✅ 함수형 인터페이스
interface Printer {
    void print(String message); // 단 하나의 추상 메서드만 있어야 함
}
```

✔ **`@FunctionalInterface` 어노테이션을 사용하면 컴파일러가 강제적으로 1개의 메서드만 허용**  
✔ **만약 여러 개의 메서드를 추가하면 오류 발생** 🚨

---

# **🚀 4️⃣ 람다식 활용 예제**

## **📌 1️⃣ 기본적인 람다식 사용**

```java
interface Printer {
    void print(String message);
}

public class LambdaExample {
    public static void main(String[] args) {
        // ✅ 람다식 사용 (Printer 인터페이스 구현)
        Printer printer = (message) -> {
            System.out.println("출력: " + message);
        };

        printer.print("Hello, Lambda!");
    }
}
```

### **🔹 실행 결과**

```plaintext
출력: Hello, Lambda!
```

✔ **인터페이스를 별도로 구현하지 않고, 람다식을 이용해 간단하게 코드 작성 가능!**

---

## **📌 2️⃣ 매개변수가 여러 개인 람다식**

```java
interface Calculator {
    int calculate(int a, int b);
}

public class LambdaExample {
    public static void main(String[] args) {
        // ✅ 덧셈
        Calculator add = (a, b) -> a + b;
        // ✅ 곱셈
        Calculator mul = (a, b) -> a * b;

        System.out.println(add.calculate(10, 20)); // 30
        System.out.println(mul.calculate(10, 20)); // 200
    }
}
```

### **🔹 실행 결과**

```plaintext
30
200
```

✔ **`return` 키워드 없이 표현식만 적어도 결과가 자동 반환됨!**  
✔ **`{}`를 생략할 수 있어 코드가 더 간결해짐** 🚀

---

# **🚀 5️⃣ 람다식 최적화**

## **📌 1️⃣ 매개변수가 1개일 때 `( )` 생략 가능**

```java
Printer printer = message -> System.out.println("출력: " + message);
```

✔ **매개변수가 하나라면 `()` 생략 가능**

---

## **📌 2️⃣ 실행문이 1개면 `{}` 생략 가능**

```java
Calculator add = (a, b) -> a + b;
```

✔ **`return`이 필요 없을 때 `{}` 생략 가능**

---

# **🚀 6️⃣ 정리**

|**항목**|**인터페이스 사용**|**람다식 사용**|
|---|---|---|
|**코드 길이**|길다 (클래스 선언 필요)|짧다 (한 줄로 표현 가능)|
|**가독성**|복잡함|간결함|
|**객체 생성 여부**|필요 (클래스 선언)|불필요 (즉시 실행 가능)|
|**사용 가능 조건**|모든 인터페이스 가능|함수형 인터페이스만 가능|

---

# **🚀 7️⃣ 결론**

✔ **인터페이스는 코드의 구조를 정의하는 역할을 하며, 객체의 동작 방식을 표준화할 때 유용**  
✔ **람다식은 "함수형 인터페이스"에서만 사용 가능하며, 코드 가독성과 유지보수를 쉽게 해줌**  
✔ **`@FunctionalInterface`를 사용하여 함수형 인터페이스를 명확하게 정의하는 것이 좋음**  
✔ **불필요한 코드 없이 짧고 직관적인 함수형 프로그래밍이 가능해짐!** 🚀

**📌 한마디로!**  
✅ **"인터페이스는 설계도를 제공하고, 람다식은 이를 간결하게 구현하는 도구!"** 🚀