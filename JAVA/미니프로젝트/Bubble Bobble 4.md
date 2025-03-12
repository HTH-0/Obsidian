# Bubble Bobble 게임 개발 기록

**📅 날짜:** 2025-03-12  
**🎮 프로젝트:** Java 기반 버블버블 게임  
**👨‍💻 개발자:** (사용자 이름)

---

## 1️⃣ 🆕 신규 추가 기능

### 📌 a. 바닥 충돌 감지 기능 추가 (`BackgroundPlayerService.java`)

- **역할:** 플레이어의 위치를 실시간으로 감지하고, 벽 및 바닥과의 충돌 여부를 판단하는 역할.
- **추가된 주요 로직:**
    - 기존에는 **왼쪽/오른쪽 벽 충돌 감지**만 존재했으나, 이번 업데이트에서 **바닥 충돌 감지 기능**이 추가됨.
    - 플레이어가 공중에 떠 있는 경우(`down` 상태) 바닥을 감지하여 더 이상 떨어지지 않도록 `setDown(false)` 처리.

```java
int bottomColorLeft = image.getRGB(player.getX() + 20, player.getY() + 50 + 5);
int bottomColorRight = image.getRGB(player.getX() + 50 - 10, player.getY() + 50 + 5);

// 하얀색 >> int 값이 -2
if(bottomColorLeft + bottomColorRight != -2) {
	// 바닥에 닿았을 때 떨어지지 않도록 처리
	player.setDown(false);
}
```

- **기대 효과:**
    - 플레이어가 공중에 있을 때만 `down()` 메서드가 작동하도록 제어하여, 자연스러운 점프 및 낙하 동작이 가능.

---

## 2️⃣ 🔄 변경된 기능

### 📌 b. 플레이어 이동 로직 개선 (`BubbleFrame.java`, `Player.java`)

- **변경 사항:**
    - `BubbleFrame.java`의 키 이벤트에서 플레이어가 벽과 충돌했을 때 이동하지 않도록 처리.
    - `Player.java`에서 `left()` 및 `right()` 메서드 실행 시 벽 충돌 여부를 먼저 확인하도록 수정.

#### 🔹 **BubbleFrame.java 키 입력 처리 수정**

```java
case KeyEvent.VK_LEFT:
	// 왼쪽 상태가 아닐 때만 이동
	if(!player.isLeft() && !player.isLeftWallCrash()) {
		player.left();						
	}
	break;
case KeyEvent.VK_RIGHT:
	if(!player.isRight() && !player.isRightWallCrash()) {
		player.right();						
	}
	break;
```

- **기대 효과:**
    - 플레이어가 벽을 통과하는 버그 방지.
    - 키 입력 시 불필요한 움직임 방지로 성능 최적화.

#### 🔹 **Player.java 이동 로직 수정**

```java
@Override
public void left() {
	left = true;
	setIcon(playerL);

	new Thread(() -> {
		while (left) {
			if(leftWallCrash) break; // 벽 충돌 시 즉시 중지
			x = x - SPEED;
			setLocation(x, y);
			try {
				Thread.sleep(10);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	}).start();
}
```

- **기대 효과:**
    - 벽과 충돌하면 즉시 이동이 멈춰서 불필요한 좌우 이동 방지.

---

## 3️⃣ 🔄 버블 생성 로직 추가 (`BubbleFrame.java`)

- **변경 사항:**
    - **스페이스바**를 누르면 플레이어의 위치에서 **버블 생성**.
    - 현재 `Bubble` 클래스가 없으므로 추후 추가할 예정.

```java
private void makeBubble(Player player) {
	Bubble bubble = new Bubble(player);
	add(bubble);
}

case KeyEvent.VK_SPACE:
	makeBubble(player);
	add(new Bubble(player));
	break;
```

- **기대 효과:**
    - 버블을 생성하는 기능이 추가될 기반을 마련.

---

## 4️⃣ 향후 개선 방향

- **Bubble 클래스 추가:** 플레이어가 버블을 발사할 수 있도록 구현 필요.
- **적 AI 적용:** 플레이어를 감지하고 이동하는 적 캐릭터 추가 예정.
- **점수 시스템 도입:** 적을 가두었을 때 점수가 증가하는 기능 추가.

---
