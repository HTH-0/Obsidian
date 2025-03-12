### 🔄 변경된 점 정리

#### 📌 **BubbleFrame.java**

1. **이벤트 리스너 추가**
    - `addEventListener()` 메서드에서 키보드 입력을 감지하는 `KeyAdapter` 추가됨.
    - `keyPressed(KeyEvent e)`에서 플레이어 이동 관련 키 (`LEFT`, `RIGHT`, `UP`, `DOWN`) 감지하여 `Player` 클래스의 `left()`, `right()`, `up()`, `down()` 메서드 호출.
    - `keyReleased(KeyEvent e)`에서 `LEFT`, `RIGHT` 키가 떼어질 경우 `setLeft(false)`, `setRight(false)`로 플레이어 이동 정지.

---

#### 📌 **Player.java**

1. **속도 조정**
    
    - `SPEED` 값이 `4`로 설정됨.
    - 점프 속도(`JUMPSPEED`)가 `2`로 설정됨.
2. **부드러운 이동 구현**
    
    - `left()`, `right()` 메서드에서 `Thread`를 사용해 부드러운 이동 추가.
    - `while(left)` 혹은 `while(right)` 루프를 실행하여 이동 지속.
3. **점프 및 낙하 기능 추가**
    
    - `up()` 메서드: 점프 동작 추가 (`for` 루프를 통해 `y` 좌표를 감소).
    - `down()` 메서드: 중력 효과를 구현하여 점프 후 낙하 동작 수행.

---
