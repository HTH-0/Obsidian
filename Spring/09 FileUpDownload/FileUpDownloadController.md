
---

# 📁 파일 업로드 컨트롤러 정리 (`FileUpDownloadController.java`)

## 📦 전체 코드

```java
package com.example.app.controller;

import java.io.File;
import java.io.IOException;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.MultipartFile;

import lombok.extern.slf4j.Slf4j;

@Controller
@Slf4j
@RequestMapping("/file")
public class FileUpDownloadController {

    private String ROOT_PATH = "c:";
    private String UPLOAD_PATH = "upload";

    // 업로드 폼
    @GetMapping("/upload")
    public void upload_form() {
        log.info("GET /file/upload..");
    }

    // 업로드 처리
    @PostMapping("/upload")
    public void upload(@RequestParam("files") MultipartFile[] files) throws IllegalStateException, IOException {
        log.info("POST /file/upload.." + files.length);

        LocalDateTime now = LocalDateTime.now();
        String folderName = now.format(DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"));

        String UploadPath = ROOT_PATH + File.separator + UPLOAD_PATH + File.separator + folderName + File.separator;

        File dir = new File(UploadPath);
        if (!dir.exists()) dir.mkdirs();

        for (MultipartFile file : files) {
            System.out.println("FILE NAME : " + file.getOriginalFilename());
            System.out.println("FILE SIZE : " + file.getSize() + " Byte");

            String filename = file.getOriginalFilename();
            File fileObject = new File(dir, filename);
            file.transferTo(fileObject); // 실제 업로드 처리
        }
    }

    // 업로드 파일 목록 보기
    @GetMapping("/list")
    public void list(Model model) {
        log.info("GET /file/list...");
        String UploadPath = ROOT_PATH + File.separator + UPLOAD_PATH + File.separator;

        File uploadDir = new File(UploadPath);
        File[] dirs = uploadDir.listFiles();

        for (File dir : dirs) {
            System.out.println("DIR : " + dir);
            for (File file : new File(dir.getPath()).listFiles()) {
                System.out.println("File : " + file);
            }
        }
        model.addAttribute("uploadPath", UploadPath);
    }
}
```

---

## 🔍 코드 분석

### ✅ 클래스 및 어노테이션 설명

- `@Controller`: Spring MVC에서 컨트롤러로 사용
    
- `@Slf4j`: 로그 출력용 Lombok 어노테이션
    
- `@RequestMapping("/file")`: `/file`로 시작하는 요청 매핑
    

### ✅ 주요 메서드 설명

- `upload_form()`
    
    - 업로드 폼을 보여주는 GET 요청 처리
        
    - 뷰 이름 생략 → `/WEB-INF/views/file/upload.jsp`와 매핑됨
        
- `upload()`
    
    - 다중 파일 업로드 처리 (`MultipartFile[]`)
        
    - 업로드 경로: `c:/upload/날짜_시간/`
        
    - 파일을 실제 디렉토리에 저장
        
- `list()`
    
    - 업로드된 파일 디렉토리 및 목록을 탐색하여 로그 출력
        
    - uploadPath 경로를 JSP에 전달 (`model.addAttribute` 사용)
        

---

## 📌 요약

- 다중 파일 업로드 처리용 컨트롤러
    
- 파일은 날짜+시간 폴더로 구분되어 `c:/upload/` 하위에 저장
    
- JSP 폼(`upload.jsp`)과 목록 뷰(`list.jsp`)와 연결됨
    

---

다음으로 `upload.jsp`와 `list.jsp`도 정리해줄게. 계속 진행할까?