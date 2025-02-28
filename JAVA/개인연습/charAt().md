### **1. `charAt()` 개요**

`charAt()` 메서드는 **Java의 `String` 클래스에서 특정 인덱스에 있는 문자(char)를 반환하는 메서드**입니다.  
배열과 마찬가지로 문자열의 인덱스는 **0부터 시작**하며, 유효한 범위를 벗어난 인덱스를 요청하면 `StringIndexOutOfBoundsException` 예외가 발생합니다.

### **2. `charAt()` 문법**

```java
char charAt(int index)
```

- `index`: 가져올 문자의 위치(0부터 시작).
- 반환값: `index` 위치에 있는 문자(`char` 타입).

---

### **3. `charAt()` 사용 예제**

```java
public class CharAtExample {
    public static void main(String[] args) {
        String text = "Hello, Java!";
        
        // 특정 인덱스의 문자 가져오기
        char firstChar = text.charAt(0); // 'H'
        char fifthChar = text.charAt(4); // 'o'
        
        System.out.println("첫 번째 문자: " + firstChar);
        System.out.println("다섯 번째 문자: " + fifthChar);
        
        // 문자열 끝 문자를 가져오기
        char lastChar = text.charAt(text.length() - 1);
        System.out.println("마지막 문자: " + lastChar);
        
        // 유효하지 않은 인덱스 접근 (예외 발생)
        // char outOfRange = text.charAt(100); // StringIndexOutOfBoundsException 발생
    }
}
```

**출력 결과:**

```
첫 번째 문자: H
다섯 번째 문자: o
마지막 문자: !
```

---

### **4. `charAt()` 활용 예제**

#### **1) 문자열에서 특정 문자 개수 세기**

아래 코드는 문자열에서 특정 문자의 개수를 세는 방법을 보여줍니다.

```java
public class CountCharacter {
    public static void main(String[] args) {
        String str = "banana";
        char target = 'a';
        int count = 0;
        
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) == target) {
                count++;
            }
        }
        
        System.out.println("'" + target + "'의 개수: " + count);
    }
}
```

**출력 결과:**

```
'a'의 개수: 3
```

#### **2) 문자열에서 모음(a, e, i, o, u) 개수 찾기**

```java
public class VowelCounter {
    public static void main(String[] args) {
        String text = "Hello Java World!";
        int vowelCount = 0;
        
        for (int i = 0; i < text.length(); i++) {
            char ch = Character.toLowerCase(text.charAt(i));
            if ("aeiou".indexOf(ch) != -1) {
                vowelCount++;
            }
        }
        
        System.out.println("모음 개수: " + vowelCount);
    }
}
```

**출력 결과:**

```
모음 개수: 5
```

---
