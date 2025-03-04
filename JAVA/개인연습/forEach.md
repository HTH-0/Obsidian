## **Java의 `for-each` 문**

### **1. 개념 설명**

`for-each` 문은 Java에서 **향상된 for 문(enhanced for loop)**이라고도 불리며, 배열이나 `Collection`(예: `ArrayList`, `HashSet`)과 같은 요소들을 순차적으로 반복(iterate)하는 데 사용됩니다.

기본 `for` 문과 달리 반복 인덱스를 직접 다룰 필요 없이, 컬렉션이나 배열의 요소를 하나씩 자동으로 순회하면서 읽을 수 있습니다.  
이로 인해 코드가 간결해지고, 가독성이 향상됩니다.

### **2. `for-each` 기본 문법**

```java
for (자료형 변수명 : 배열 또는 컬렉션) {
    // 반복할 코드
}
```

- `자료형 변수명` : 배열이나 컬렉션의 각 요소를 저장할 변수입니다.
- `배열 또는 컬렉션` : 순회할 대상입니다.

---

### **3. 예제 코드**

#### **(1) 배열에서 `for-each` 사용**

```java
public class ForEachExample {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30, 40, 50};

        // for-each 문 사용
        for (int num : numbers) {
            System.out.println(num);
        }
    }
}
```

#### **출력**

```
10
20
30
40
50
```

---

#### **(2) `ArrayList`에서 `for-each` 사용**

```java
import java.util.ArrayList;

public class ForEachListExample {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Charlie");

        // for-each 문 사용
        for (String name : names) {
            System.out.println(name);
        }
    }
}
```

#### **출력**

```
Alice
Bob
Charlie
```

---

#### **(3) `HashSet`에서 `for-each` 사용**

```java
import java.util.HashSet;

public class ForEachSetExample {
    public static void main(String[] args) {
        HashSet<Double> prices = new HashSet<>();
        prices.add(19.99);
        prices.add(25.50);
        prices.add(10.75);

        // for-each 문 사용
        for (double price : prices) {
            System.out.println(price);
        }
    }
}
```

#### **출력 (순서는 보장되지 않음)**

```
25.5
10.75
19.99
```

---

### **4. `for-each`의 한계**

1. **인덱스 사용 불가**
    
    - 기존 `for` 문과 달리 인덱스를 사용할 수 없기 때문에, 특정 요소에 접근하거나 수정해야 할 경우 `for-each` 문을 사용할 수 없습니다.
2. **요소 수정 불가능 (Primitive 제외)**
    
    - `for-each` 문은 읽기 전용입니다. 즉, 컬렉션의 요소를 직접 수정할 수 없습니다.
    - 단, `int[]` 같은 **배열의 원시 타입(primitive type)** 요소는 수정이 가능함.
3. **반복을 중간에 종료하는 것은 가능하지만, 요소를 제거할 수 없음**
    
    - `break` 또는 `return`으로 반복을 중단할 수 있지만, 컬렉션에서 요소를 삭제하려면 `Iterator`를 사용해야 함.

---

### **5. `for-each` 연습문제**

#### **(1) 초급 문제: 배열 출력**

다음 `for-each` 문을 사용하여 정수 배열의 모든 요소를 출력하는 프로그램을 작성하세요.

```java
public class ForEachPractice1 {
    public static void main(String[] args) {
        int[] numbers = {5, 10, 15, 20, 25};

        // TODO: for-each 문을 사용하여 배열 출력
    }
}
```

---

#### **(2) 고급 문제: 특정 조건을 만족하는 요소 찾기**

문자열 리스트에서 길이가 5 이상인 문자열만 출력하는 프로그램을 `for-each` 문을 사용하여 작성하세요.

```java
import java.util.ArrayList;

public class ForEachPractice2 {
    public static void main(String[] args) {
        ArrayList<String> words = new ArrayList<>();
        words.add("Java");
        words.add("Programming");
        words.add("Loop");
        words.add("Enhancement");

        // TODO: for-each 문을 사용하여 길이가 5 이상인 단어만 출력
    }
}
```
