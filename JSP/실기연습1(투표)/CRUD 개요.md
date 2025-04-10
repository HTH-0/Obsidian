# 📄 JSP 파일 기능 정리

---

## 🗂 `index.jsp`

### ✔️ 역할

후보자 전체 목록을 조회하여 **테이블 형태로 출력**

### 🧩 주요 로직

- `OracleDBUtils.getInstance().selectAllMember()` 호출
    
- 후보자 목록을 `List<MemberDto>`로 가져옴
    
- 각 후보자의 학력(`p_school`) 값은 숫자로 저장되어 있어 `switch`문으로 다음과 같이 변환 출력
    
    - `"1"` → 고졸
        
    - `"2"` → 학사
        
    - `"3"` → 석사
        
    - `"4"` → 박사
        

### 🧱 출력 테이블 구성

- 후보번호 (`m_no`)
    
- 성명 (`m_name`)
    
- 정당 (`p_name`)
    
- 학력 (`p_school`, switch로 변환)
    
- 주민번호 (`m_jumin`)
    
- 지역 (`m_city`)
    
- 대표전화 (`p_tel1-p_tel2-p_tel3`)
    

### 📦 기타

- `Header.jsp`, `Nav.jsp`, `Footer.jsp` 인클루드 되어 전체 레이아웃 구성
    

---

## 🗂 `create.jsp`

### ✔️ 역할

**투표 정보 등록 처리** 수행

### 🧩 주요 흐름

#### 1. 파라미터 직접 수신 방식

```jsp
String jumin = request.getParameter("v_jumin");
String name = request.getParameter("v_name");
...
VoteDto voteDto = new VoteDto(jumin, name, ...);
```

#### 2. 액션 태그 방식 (자동 매핑)

```jsp
<jsp:useBean id="voteDto2" class="Utils.VoteDto" scope="request" />
<jsp:setProperty name="voteDto2" property="*" />
```

#### 3. DB 저장

```java
int result = OracleDBUtils.getInstance().insertVote(voteDto2);
```

- 저장 성공 시 → `read.jsp`로 forward
    
- 저장 실패 시 → alert 후 `index.jsp`로 이동
    

### 📦 기타

- `voteDto`와 `voteDto2`를 모두 출력 (디버깅용 `System.out.println`)
    
- 인코딩 설정 (`UTF-8`) 포함되어 한글 처리 문제 방지
    

---
