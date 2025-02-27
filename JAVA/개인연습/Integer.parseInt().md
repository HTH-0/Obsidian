---
aliases:
  - "**`Integer.parseInt()"
---
### **`Integer.parseInt()` 개념 및 사용법**

#### **1. 개념**

`Integer.parseInt()`는 `String` 형태로 저장된 숫자를 정수(`int`)로 변환하는 정적(static) 메서드입니다. `java.lang.Integer` 클래스에 속해 있으며, 주어진 문자열이 숫자로 변환될 수 있는 경우에만 사용 가능합니다.

#### **2. 사용법**

```java
int number = Integer.parseInt("123");
System.out.println(number); // 출력: 123
```

위 코드에서 `"123"`이라는 문자열을 `int`로 변환하여 변수 `number`에 저장하였습니다.

#### **3. 오버로드된 메서드**

`Integer.parseInt()`는 두 가지 버전이 있습니다.

1. **기본 10진수 변환**
    
    ```java
    int num = Integer.parseInt("456");
    System.out.println(num); // 출력: 456
    ```
    
2. **기수(radix)를 지정한 변환**
    
    ```java
    int binary = Integer.parseInt("1010", 2); // 2진수(바이너리) → 10진수 변환
    System.out.println(binary); // 출력: 10
    ```
    

#### **4. 예제 코드**

##### **(1) 문자열을 정수로 변환하는 기본 예제**

```java
public class ParseIntExample {
    public static void main(String[] args) {
        String strNumber = "2024";
        int number = Integer.parseInt(strNumber);
        System.out.println("변환된 정수 값: " + number);
    }
}
```

**출력:**

```
변환된 정수 값: 2024
```

##### **(2) 16진수(HEX) 문자열 변환**

```java
public class HexParseExample {
    public static void main(String[] args) {
        String hexNumber = "A3";
        int decimal = Integer.parseInt(hexNumber, 16); // 16진수를 10진수로 변환
        System.out.println("16진수 A3의 10진수 값: " + decimal);
    }
}
```

**출력:**

```
16진수 A3의 10진수 값: 163
```

##### **(3) 잘못된 문자열 변환 시 예외 발생**

```java
public class ParseIntErrorExample {
    public static void main(String[] args) {
        String invalidNumber = "12A4"; // 숫자가 아닌 문자 포함

        try {
            int number = Integer.parseInt(invalidNumber);
            System.out.println("변환된 숫자: " + number);
        } catch (NumberFormatException e) {
            System.out.println("변환 실패! 숫자가 아닌 문자가 포함되어 있습니다.");
        }
    }
}
```

**출력:**

```
변환 실패! 숫자가 아닌 문자가 포함되어 있습니다.
```

(`NumberFormatException` 예외가 발생하며, 올바른 숫자 문자열이 아니면 변환이 불가능합니다.)

---

### **5. 연습 문제**

#### **문제 1: 문자열을 정수로 변환하기** _(초급)_

사용자로부터 숫자 문자열을 입력받아 `Integer.parseInt()`를 이용해 정수로 변환하고 출력하는 프로그램을 작성하세요.

##### **예제 입력 및 출력**

```
입력: "150"
출력: 변환된 정수 값: 150
```

---

#### **문제 2: 기수 변환 프로그램 만들기** _(중급)_

사용자가 직접 변환할 진법(2~36)을 입력하면 해당 진법에 따라 문자열을 정수로 변환하는 프로그램을 작성하세요.

##### **예제 입력 및 출력**

```
입력: "1101"
기수(2~36): 2
출력: 변환된 정수 값: 13
```

위 문제들을 직접 해결해 보면서 `Integer.parseInt()`의 동작을 익혀 보세요! 🚀