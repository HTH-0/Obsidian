### **`@Override`의 범위와 역할**

`@Override` 어노테이션은 **부모 클래스(또는 인터페이스)의 메서드를 자식 클래스에서 올바르게 재정의(Override)했는지 확인하는 역할**을 합니다.

---

### **📌 `@Override`의 적용 범위**

- **부모 클래스의 메서드를 자식 클래스에서 재정의(Overriding)할 때 사용 가능**
- **메서드 이름, 매개변수, 반환 타입이 부모 클래스와 동일해야 함**
- **접근 제어자는 부모보다 좁게 설정할 수 없음**
- **부모 클래스에 존재하지 않는 메서드에 `@Override`를 사용하면 오류 발생**

---

### **📌 `@Override`가 적용된 코드 분석**

```java
class Appliance {
    void powerOn() {
        System.out.println("전원 on");
    }

    void powerOff() {
        System.out.println("전원 off");
    }
}

class Tv extends Appliance {
    
    @Override  // ✅ 부모 클래스의 powerOn()을 올바르게 오버라이딩함
    void powerOn() {
        System.out.println("Tv를 켭니다.");
    }

    void powerOff() {  // ❌ @Override 없음 → 하지만 오버라이딩은 됨
        System.out.println("Tv를 끕니다.");
    }

    void ChannelChange() {  // ✅ 부모 클래스에 없는 새로운 메서드 (오버라이딩 아님)
        System.out.println("채널을 바꿉니다.");
    }
}

public class P10Upcasting {
    public static void main(String[] args) {
        Appliance t1 = new Tv();  // 업캐스팅
        t1.powerOn();   // "Tv를 켭니다." 출력 (오버라이딩된 메서드 실행)
        t1.powerOff();  // "Tv를 끕니다." 출력 (오버라이딩된 메서드 실행)

        // t1.ChannelChange(); ❌ (오류 발생: 부모 클래스에 존재하지 않는 메서드는 호출 불가능)
    }
}
```

### **📌 `@Override`가 적용된 범위**

- `powerOn()`은 부모 클래스에 존재하는 메서드이므로, 자식 클래스 `Tv`에서 **재정의 가능** → `@Override` 적용 가능 ✅
- `powerOff()`도 부모 클래스에 존재하는 메서드이므로, **재정의 가능**하지만 `@Override`를 생략했음 → 오류는 없지만 추가하는 것이 권장됨
- `ChannelChange()`는 부모 클래스에 존재하지 않는 새로운 메서드이므로 **오버라이딩이 아님** → `@Override`를 붙이면 **컴파일 오류 발생**

---

### **📌 `@Override`를 사용하지 않으면?**

- 오버라이딩이 정상적으로 되었는지 **컴파일러가 확인하지 않음**
- 실수로 **메서드 이름을 틀려도 오류가 발생하지 않음**
- `@Override`를 사용하면 **부모 클래스의 메서드와 정확히 일치해야 하므로 실수를 방지**할 수 있음

```java
class Tv extends Appliance {
    @Override
    void powerOn() {  // 부모 클래스에 존재 → 올바른 오버라이딩
        System.out.println("Tv를 켭니다.");
    }

    // 실수로 powerOff() 이름을 잘못 입력했을 경우
    void PowerOff() {  // ❌ 부모 클래스의 powerOff()와 다름 (대소문자 차이)
        System.out.println("Tv를 끕니다.");
    }
}
```

📌 이 경우 `PowerOff()` 메서드는 부모 클래스의 `powerOff()`를 재정의한 것이 아니라, **새로운 메서드**로 인식됩니다.  
✅ `@Override`를 붙였다면 **컴파일 오류가 발생하여 실수를 방지**할 수 있습니다.

---

### **📌 `@Override` 사용 시 주의할 점**

1. **메서드 이름, 매개변수, 반환 타입이 부모 클래스와 정확히 일치해야 함**
2. **접근 제어자는 부모 클래스보다 좁아지면 안 됨**
3. **부모 클래스에 없는 메서드에는 사용 불가능**

---

### **🚀 정리**

- ✅ `@Override`는 **부모 클래스의 메서드를 자식 클래스에서 재정의할 때 사용**
- ✅ 오버라이딩이 **정확하게 이루어졌는지 컴파일러가 체크**하여 실수를 방지할 수 있음
- ✅ 부모 클래스에 없는 메서드에 `@Override`를 사용하면 **컴파일 오류 발생**
- ✅ **가능한 모든 오버라이딩 메서드에 `@Override`를 붙이는 것이 권장됨** (실수 방지)

---
