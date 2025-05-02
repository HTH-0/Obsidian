
---

# ✅ Step 5. JavaScript – Axios를 활용한 비동기 요청 처리

## 📦 사용된 CDN

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.6.8/axios.min.js" crossorigin="anonymous"></script>
```

---

## 📌 기본 변수

```javascript
const projectPath = '${pageContext.request.contextPath}';
```

→ context root (`/app` 등)를 동적으로 얻어 API URL을 구성하는 데 사용

---

## 🧪 주요 요청 구현 예시

### ✅ 1. GET 방식 (쿼리 파라미터 전달)

```javascript
const axiosAsyncGetBtn = document.querySelector('.axiosAsyncGetBtn');
axiosAsyncGetBtn.addEventListener('click', function() {
    const form = document.axiosAsyncGetForm;
    axios.get(projectPath + "/memo/add_rest_get?id=" + form.id.value + "&text=" + form.text.value)
         .then(resp => console.log(resp))
         .catch(err => console.log(err));
});
```

### ✅ 2. POST 방식 (JSON Body 전달)

```javascript
const axiosAsyncPostBtn = document.querySelector('.axiosAsyncPostBtn');
axiosAsyncPostBtn.addEventListener('click', function() {
    const form = document.axiosAsyncPostForm;
    const param = {
        "id": form.id.value,
        "text": form.text.value
    };
    const headers = { 'Content-Type': 'application/json' };

    axios.post(projectPath + "/memo/add_rest_post", param, headers)
         .then(resp => console.log(resp))
         .catch(err => console.log(err));
});
```

### ✅ 3. PUT 방식 (PathVariable 사용)

```javascript
const axiosAsyncPutBtn = document.querySelector('.axiosAsyncPutBtn');
axiosAsyncPutBtn.addEventListener('click', function() {
    const form = document.axiosAsyncPutForm;
    axios.put(projectPath + "/memo/put/" + form.id.value + "/" + form.text.value)
         .then(resp => console.log(resp))
         .catch(err => console.log(err));
});
```

### ✅ 4. DELETE 방식 (PathVariable 사용)

```javascript
const axiosAsyncDeleteBtn = document.querySelector('.axiosAsyncDeleteBtn');
axiosAsyncDeleteBtn.addEventListener('click', function() {
    const form = document.axiosAsyncDeleteForm;
    axios.delete(projectPath + "/memo/remove/" + form.id.value)
         .then(resp => console.log(resp))
         .catch(err => console.log(err));
});
```

---

## 💡 기타 처리 예정

- `PATCH` 메서드는 UI 구성은 되어 있으나 실제 요청 처리 스크립트는 미구현 상태
    

---

## 📌 요약

- Axios는 RESTful API와의 통신을 가장 간편하게 처리할 수 있는 라이브러리
    
- `GET`, `POST`, `PUT`, `DELETE` 각각에 대해 명확하게 테스트할 수 있도록 구현되어 있음
    
- `projectPath`를 활용해 JSP 환경에서도 경로 충돌 없이 사용 가능
    
- 요청 결과는 콘솔에 로그로 출력되며, 이후 `.then()`에서 화면 렌더링 확장 가능
    

---
