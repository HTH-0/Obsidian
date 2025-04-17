## 📝 To-Do List 프로젝트 로그 (파일 저장 기능 추가)

---

## 1️⃣ 프로젝트 개요

- **프로젝트명:** `Todo List (파일 저장 기능 추가)`
- **프로젝트 목적:** 사용자의 할 일 목록을 관리하고 프로그램 종료 후에도 데이터를 유지할 수 있도록 파일 저장 기능을 추가
- **프로그램 설명:** 사용자가 할 일을 추가, 삭제, 조회할 수 있으며, 이를 파일(`TodoList.txt`)에 저장하여 프로그램 종료 후에도 데이터가 유지됨

---

## 2️⃣ 시스템 설계 및 코드 구조

- **주요 기능 및 역할**
    
    - `할 일 추가`: 사용자가 새로운 할 일을 입력하면 리스트에 추가하고 파일에 저장
    - `리스트 삭제`: 특정 인덱스의 할 일을 삭제하고 파일을 업데이트
    - `할 일 목록 조회`: 현재 저장된 할 일 목록을 조회
    - `데이터 저장`: 할 일 추가 및 삭제 시 파일(`TodoList.txt`)에 자동으로 저장
    - `데이터 불러오기`: 프로그램 실행 시 기존 파일에서 데이터를 불러옴
- **기능별 주요 클래스 및 역할**
    
    - `Todo2`: 프로그램의 실행 및 사용자 입력 처리
    - `Save()`: `TodoList.txt` 파일에 데이터를 저장
    - `Load()`: `TodoList.txt` 파일에서 기존 데이터를 불러옴

---

## 3️⃣ 주요 코드 및 기능 설명

### ✨ **할 일 추가 기능 (파일 저장 포함)**

```java
case 1:
    while (true) {
        System.out.println("추가 할 내용 : ");
        String task = sc.nextLine();
        if (task.equals("exit")) {
            break;
        }
        TodoList.add(task);
        Save();  // 파일 저장
        System.out.println("추가되었습니다 (나가기 : exit)");
    }
    continue;
```

📌 **설명:**

- 사용자로부터 새로운 할 일을 입력받고 `TodoList`에 추가
- `Save()` 메서드를 호출하여 즉시 파일에 저장
- `exit`을 입력하면 추가 모드 종료

---

### ✨ **리스트 삭제 기능 (파일 업데이트 포함)**

```java
case 2:
    System.out.println("삭제하고 싶은 번호를 입력해주세요");
    for (int i = 1; i <= TodoList.size(); i++) {
        System.out.println(i + "." + TodoList.get(i - 1));
    }
    System.out.println("번호 선택 : ");
    int num = sc.nextInt() - 1;

    if (num >= 0 && num < TodoList.size()) {
        TodoList.remove(num);
        Save();  // 파일 업데이트
        System.out.println("삭제되었습니다");
    } else {
        System.out.println("번호를 다시 입력해주세요");
    }
    break;
```

📌 **설명:**

- 삭제할 항목의 번호를 입력받아 `TodoList`에서 제거
- `Save()`를 호출하여 변경 사항을 파일에 반영

---

### ✨ **할 일 목록 조회 기능**

```java
case 3:
    System.out.println("📄 List ");
    if (TodoList.isEmpty()) {
        System.out.println("리스트가 비어있습니다\n할 일을 추가해주세요");
    } else {
        for (int i = 1; i <= TodoList.size(); i++) {
            System.out.println(i + "." + TodoList.get(i - 1));
        }
    }
    break;
```

📌 **설명:**

- `TodoList`를 순회하며 저장된 할 일 목록을 출력
- 리스트가 비어 있을 경우 메시지 출력

---

### ✨ **파일 저장 기능 (`Save()`)**

```java
private static void Save() {
    try (BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_NAME))) {
        for (String task : TodoList) {
            writer.write(task);
            writer.newLine();
        }
    } catch (IOException e) {
        System.out.println("저장 중 오류 발생 : " + e.getMessage());
    }
}
```

📌 **설명:**

- `BufferedWriter`를 사용하여 `TodoList.txt` 파일에 할 일 목록을 저장
- `newLine()`을 사용해 한 줄씩 저장하여 데이터 가독성을 유지

---

### ✨ **파일 불러오기 기능 (`Load()`)**

```java
private static void Load() {
    File file = new File(FILE_NAME);
    if (file.exists()) {
        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
            String line;
            while ((line = reader.readLine()) != null) {
                TodoList.add(line);
            }
        } catch (IOException e) {
            System.out.println("불러오기 중 오류 발생 : " + e.getMessage());
        }
    }
}
```

📌 **설명:**

- 프로그램 실행 시 `TodoList.txt`에서 기존 데이터를 읽어와 `TodoList`에 저장
- `BufferedReader`를 사용하여 파일을 한 줄씩 읽어 처리

---

## 4️⃣ 문제 해결 과정

- **문제점:** 프로그램 종료 후 기존 목록이 초기화되는 문제
    - **해결책:** 파일 저장 및 불러오기 기능을 추가하여 데이터가 지속되도록 개선
- **문제점:** 삭제 후 파일이 자동으로 업데이트되지 않음
    - **해결책:** 할 일을 삭제할 때마다 `Save()`를 호출하여 변경 사항을 파일에 반영
- **문제점:** 사용자 입력 오류 발생 가능 (잘못된 번호 입력 등)
    - **해결책:** 삭제 기능에서 번호 검증을 추가하여 범위 초과 입력 방지

---

## 5️⃣ 향후 개선 방향

- **입력 오류 방지 기능 추가** (예외 처리 추가)
- **UI 개선** (번호 선택을 더 직관적으로 변경)
- **데이터베이스 연동** (파일 대신 DB 활용하여 안정적인 데이터 저장)
- **완료된 할 일 관리 기능 추가** (할 일 완료 여부를 표시하는 기능 추가)

---