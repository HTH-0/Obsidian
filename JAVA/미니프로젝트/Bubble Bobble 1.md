### **Bubble Bobble 게임 개발 기록**

**📅 날짜:** [2025-03-12]  
**🎮 프로젝트:** Java 기반 버블버블 게임  


---

## **1️⃣ 프로젝트 개요**

- Java Swing을 활용하여 Bubble Bobble 스타일의 게임 개발
- 플레이어 이동 및 키 입력 감지를 포함한 기본 게임 프레임워크 구현

---

## **2️⃣ 코드 분석**

### 📌 a. 게임 프레임 (`BubbleFrame.java`)

- **역할:** 게임의 전체 프레임을 관리하고, 키 입력 이벤트 처리
- **주요 코드:**
    
    ```java
    public class BubbleFrame extends JFrame {
        private JLabel backgroundMap;
        private Player player;
        
        public BubbleFrame() {
            initData();
            setInitLayout();
            addEventListener();
        }
        
        private void addEventListener() {
            this.addKeyListener(new KeyAdapter() {
                @Override
                public void keyPressed(KeyEvent e) {
                    switch(e.getKeyCode()) {
                        case KeyEvent.VK_LEFT:
                            player.left();
                            break;
                        case KeyEvent.VK_RIGHT:
                            player.right();
                            break;
                        case KeyEvent.VK_UP:
                            player.up();
                            break;
                        case KeyEvent.VK_DOWN:
                            player.down();
                            break;
                    }
                }
            });
        }
    }
    ```
    
- `JFrame`을 확장하여 게임 창을 생성하고, 키 이벤트를 감지하여 `Player` 객체의 이동을 처리

---

### 📌 b. 메인 실행 (`GameTest1.java`)

- **역할:** `BubbleFrame`을 실행하여 게임을 시작하는 역할
- **주요 코드:**
    
    ```java
    public class GameTest1 {
        public static void main(String[] args) {
            new BubbleFrame();
        }
    }
    ```
    
- 단순히 `BubbleFrame` 객체를 생성하여 게임을 실행

---

### 📌 c. 이동 가능 인터페이스 (`Moveable.java`)

- **역할:** 이동 관련 기능을 정의하는 인터페이스
- **주요 코드:**
    
    ```java
    public interface Moveable {
        public abstract void left();
        public abstract void right();
        public abstract void up();
        public abstract void down();
    }
    ```
    
- `Player` 클래스에서 구현해야 할 이동 동작을 정의

---

### 📌 d. 플레이어 (`Player.java`)

- **역할:** 플레이어의 이동 및 이미지 변경 관리
- **주요 코드:**
    
    ```java
    public class Player extends JLabel implements Moveable {
        private int x;
        private int y;
        private ImageIcon playerR, playerL;
        
        public Player() {
            initData();
            setInitLayout();
        }
        
        private void initData() {
            playerR = new ImageIcon("img/playerR.png");
            playerL = new ImageIcon("img/playerL.png");
            x = 55;
            y = 535;
            setIcon(playerR);
            setSize(50, 50);
            setLocation(x, y);
        }
        
        @Override
        public void left() {
            setIcon(playerL);
            x -= 10;
            setLocation(x, y);
        }
        
        @Override
        public void right() {
            setIcon(playerR);
            x += 10;
            setLocation(x, y);
        }
        
        @Override
        public void up() {
            y -= 10;
            setLocation(x, y);
        }
        
        @Override
        public void down() {
        }
    }
    ```
    
- `Moveable` 인터페이스를 구현하여 `left()`, `right()`, `up()` 등의 이동 기능을 수행
- 이미지 변경을 통해 플레이어가 움직일 때 방향이 바뀌도록 처리

---

## **3️⃣ 발견된 문제점 및 개선 사항**

✅ **발견된 문제**

- `down()` 메서드가 구현되지 않음
- 점프 기능(`up()`)이 단순히 위치 변경만 수행 (물리적 효과 X)
- 플레이어 이동이 프레임 경계를 벗어나는지 검사하는 로직 부재

🛠 **개선 사항**

- `down()` 구현: 중력 효과를 추가하여 점프 후 자연스럽게 내려오도록 변경
- **경계 검사**: 이동 시 `if` 조건을 추가하여 프레임 밖으로 벗어나지 않도록 설정
- **애니메이션 추가**: `Thread` 또는 `Timer`를 사용하여 부드러운 이동 구현

---

## **4️⃣ 향후 개발 계획**

🚀 **추가해야 할 기능**

- **버블 발사 기능**: 플레이어가 공격할 수 있도록 추가
- **적 AI 구현**: 랜덤 이동 및 플레이어 감지 시스템 구축
- **충돌 감지 시스템**: 버블과 적의 충돌을 판별하여 게임 로직 확장
- **점수 시스템**: 적을 가두었을 때 점수 증가

✅ **다음 목표**

1. 중력 효과 추가하여 자연스러운 점프 구현
2. 버블 시스템 추가하여 적을 가둘 수 있도록 개발
3. 적 캐릭터와 충돌 감지를 통해 게임 진행 로직 개선

---
