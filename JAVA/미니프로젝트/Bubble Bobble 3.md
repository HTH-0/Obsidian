# Bubble Bobble 게임 개발 기록

**📅 날짜:** 2025-03-12  
**🎮 프로젝트:** Java 기반 버블버블 게임 

---

## 1️⃣ 🆕 신규 추가 기능

### 📌 a. 배경 충돌 감지 (`BackgroundPlayerService.java`)

- **역할:** 플레이어의 위치를 감지하고 배경 이미지 픽셀의 색상 정보를 분석하여 벽과의 충돌 여부를 판단함.
- **추가된 주요 로직:**
    - `run()` 메서드를 실행하면서 지속적으로 플레이어 위치의 왼쪽과 오른쪽 색상을 확인.
    - 벽과 충돌하면 `leftWallCrash` 또는 `rightWallCrash` 값을 `true`로 설정하고, 플레이어 이동을 중지.

```java
public void run() {
	while(true) {
		Color leftColor = new Color(image.getRGB(player.getX()+10, player.getY()+25));
		Color rightColor = new Color(image.getRGB(player.getX()+50+10, player.getY()+25));
	
		if(leftColor.getRed() == 255 && leftColor.getGreen() == 0 && leftColor.getBlue() == 0) {
			System.out.println("왼쪽 벽과 충돌");
			player.setLeftWallCrash(true);
			player.setLeft(false);
		} else if(rightColor.getRed() == 255 && rightColor.getGreen() == 0 && rightColor.getBlue() == 0) {
			System.out.println("오른쪽 벽과 충돌");
			player.setRightWallCrash(true);
			player.setRight(false);
		} else {
			player.setLeftWallCrash(false);
			player.setRightWallCrash(false);
		}
	}
}
```

- **기대 효과:** 플레이어가 벽과 충돌할 경우 이동 방향이 즉시 멈추게 되어 벽을 뚫고 지나가는 문제를 방지.

---

## 2️⃣ 🔄 변경된 기능

### 📌 b. 게임 프레임 초기화 (`BubbleFrame.java`)

- **변경 사항:**
    - 게임이 시작될 때 `BackgroundPlayerService` 스레드를 실행하여 플레이어의 움직임을 감지하도록 변경.

```java
public BubbleFrame() {
	initData();
	setInitLayout();
	addEventListener();
	
	// 해당 클래스 시작
	new Thread(new BackgroundPlayerService(player)).start();
}
```

- **기대 효과:**
    - 플레이어의 이동이 실시간으로 감지되며, 벽 충돌이 발생하면 즉시 이동을 차단하여 보다 자연스러운 게임 플레이가 가능.

---

### 📌 c. 플레이어 이동 (`Player.java`)

- **변경 사항:**
    - 플레이어 이동 중 **벽 충돌 여부를 확인**하여, 충돌 상태에서 이동 방향을 유지하지 않도록 변경됨.

```java
@Override
public void left() {
	left = true;
	setIcon(playerL);

	new Thread(() -> {
		while (left) {
			if(leftWallCrash) break; // 벽 충돌 감지 시 중지
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
    - 플레이어가 벽과 충돌할 경우 즉시 이동이 멈추도록 개선됨.

---

## 3️⃣ 향후 개선 방향

- **플레이어 점프 시 벽 충돌 감지 개선**: 현재 `up()` 동작 중에는 벽 충돌이 반영되지 않음. 이를 해결하기 위해 `BackgroundPlayerService`에서 점프 시 충돌 감지를 추가할 필요 있음.
- **적 AI 적용 예정**: 플레이어의 움직임을 감지하고 추적하는 적 캐릭터 구현 예정.
- **버블 발사 기능 추가**: 플레이어가 버블을 발사하여 적을 가둘 수 있도록 기능 추가 예정.

---
