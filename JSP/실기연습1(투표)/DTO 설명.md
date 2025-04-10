# 📦 클래스별 역할 및 기능 정리

---

## 🧾 MemberDto.java

### ✔️ 역할

- **후보자(정당 포함) 정보**를 저장하는 DTO (Data Transfer Object)
    

### 🧩 주요 필드

- `m_no`: 후보자 번호
    
- `m_name`: 후보자 이름
    
- `p_name`: 정당 이름
    
- `p_school`: 학력
    
- `m_jumin`: 주민등록번호
    
- `m_city`: 지역구
    
- `p_tel1`, `p_tel2`, `p_tel3`: 정당 전화번호 (3분할 저장)
    

### 🛠️ 기능

- 모든 필드에 대해 `getter/setter` 제공
    
- 기본 생성자 + 전체 인자를 받는 생성자
    
- 객체 내용을 확인하기 위한 `toString()` 오버라이딩
    

---

## 🗳️ VoteDto.java

### ✔️ 역할

- **투표자 정보 및 투표 데이터**를 저장하는 DTO
    

### 🧩 주요 필드

- `v_jumin`: 투표자 주민번호
    
- `v_name`: 투표자 이름
    
- `m_no`: 투표한 후보 번호
    
- `v_time`: 투표 시간
    
- `v_area`: 투표 장소
    
- `v_confirm`: 투표 여부 (확인 여부)
    

### 🛠️ 기능

- 모든 필드에 대해 `getter/setter` 제공
    
- 기본 생성자 + 전체 인자를 받는 생성자
    
- 객체 출력용 `toString()` 오버라이딩
    

---

## 🛠️ DBUtils.java

### ✔️ 역할

- **DB 연결 및 데이터 조회/삽입을 처리하는 유틸 클래스**
    
- **싱글톤 패턴 적용**으로 인스턴스 하나만 생성되도록 제한
    

### 🔗 연결 정보

- Oracle DB 연결 (JDBC URL: `jdbc:oracle:thin:@localhost:1521:xe`)
    
- 사용자 ID: `system`, 비밀번호: `1234`
    

### 📌 주요 기능

#### 1. `selectAllMember()`

- `TBL_MEMBER_202005` 테이블과 `TBL_PARTY_202005` 테이블 조인
    
- 모든 후보자 정보 조회하여 `List<MemberDto>`로 반환
    

#### 2. `insertVote(VoteDto dto)`

- `TBL_VOTE_202005` 테이블에 한 명의 투표 정보 입력
    
- 성공 시 `1`, 실패 시 `0` 반환
    

#### 3. `selectAllVote()`

- `TBL_VOTE_202005` 테이블의 전체 투표 데이터를 조회
    
- `List<VoteDto>`로 반환
    

### 📌 기타 특징

- 모든 DB 작업은 `Connection`, `PreparedStatement`, `ResultSet`을 통해 처리
    
- 실행 후에는 `close()`로 자원 정리
    

---
