---
description: Day 1 Multipart Formdata
---

# Day 1 Multipart Formdata

### HTML에서 파일을 전송하는 방법

1. HTML 태그 레벨
   1. `<input type="file">`
   2. `<form enctype="multipart/form-data">`
2. 전송 레벨
   1. Form 태그 (ex: `<form .... method="post">`)&#x20;
   2. AJAX  (fetch, axios 등 JS 라이브러리 사용) -> 단, 백엔드에서 CORS 설정이 되어 있어야 함
3. Spring
   1. Controller에서 변수에 `@ModelAttribute` 을 달아서 데이터 수신
      * `@ModelAttribue`의 기능
        1. HTTP Body 내용 감지  (Form data, bytes 등 가능)
        2. HTTP Parameter 내용 감지
   2. UploadImageForm(이쪽이 보다 혼동의 여지가 적음) 또는 UploadImageDto 정도의 네이밍으로 클래스 또는 레코드를 생성하여 데이터를 받아들임
   3. 파일을 받아서 처리하기
      1. 파일이름 : 임의로 처리할 경우 TSID 등을 활용
      2. File 객체를 만들어서 쓸 때는 FileOutputStream을 생성함
