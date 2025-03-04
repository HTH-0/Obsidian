`private` 접근 제한자를 사용하는 이유는 **캡슐화(encapsulation)**를 통해 **데이터 보호 및 코드 구조 유지**를 하기 위함입니다.

### ✅ **이 코드에서 `private`을 사용하는 이유**

```java
private static final String FILE_NAME = "TodoList.txt"; 
private static ArrayList<String> TodoList = new ArrayList<>();
```

#### 1️⃣ **직접적인 접근을 막아 불필요한 수정 방지**

- `FILE_NAME`은 `final`로 선언된 **상수**이므로, 변경될 일이 없습니다.
- `TodoList`는 할 일 목록을 저장하는 핵심 데이터로, 외부에서 직접 접근하면 **예기치 않은 수정이 발생할 수 있음**.
    - 만약 `private` 없이 `public static ArrayList<String> TodoList`로 선언하면, **다른 클래스에서도 직접 접근하여 수정할 수 있음**.
    - 잘못된 사용 예:
        
        ```java
        Todo.TodoList.clear(); // 외부에서 직접 할 일 목록을 삭제 가능!
        ```
        

#### 2️⃣ **객체 지향 프로그래밍(OOP) 원칙 적용**

- OOP에서 중요한 **정보 은닉(information hiding)**을 실천하여 코드의 유지보수성을 높임.
- 외부 클래스에서 `TodoList`를 직접 변경하지 못하게 하고, 오직 `saveToFile()`과 같은 메서드를 통해서만 안전하게 수정 가능.

#### 3️⃣ **일관된 데이터 처리 보장**

- `saveToFile()`과 `loadFromFile()`을 통해 데이터가 항상 파일과 동기화됨.
- 만약 `TodoList`가 `public`이라면, 외부에서 직접 데이터를 조작하여 프로그램 흐름을 깨뜨릴 수 있음.

---

### 🔍 **`private` 없이 사용했을 때 문제점**

```java
public static ArrayList<String> TodoList = new ArrayList<>();
```

- 다른 클래스에서 `TodoList`를 직접 수정하면, **파일 저장 로직이 동작하지 않을 가능성이 있음**.
- 예를 들어, 외부 클래스에서 다음처럼 실행하면:
    
    ```java
    Todo.TodoList.add("테스트");  // 추가했지만 saveToFile() 호출되지 않음 → 파일에는 저장되지 않음!
    ```
    
    - 메모리 상에는 추가되지만, 파일과 동기화되지 않아 **데이터 손실 가능성** 발생.

---

### ✅ **결론**

👉 **`private`을 사용하면 데이터를 보호하고, 코드의 일관성을 유지하며, 불필요한 외부 접근을 차단할 수 있다.**  
👉 외부에서는 `saveToFile()` 및 `loadFromFile()`을 통해서만 데이터가 변경되도록 강제할 수 있다.