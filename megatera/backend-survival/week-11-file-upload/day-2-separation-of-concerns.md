---
description: Day 2 Separation of Concerns
---

# Day 2 Separation of Concerns

### 파일 업로드 기능 구현 시 관심사 분리가 필요한 이유

* 1강에서 다룬 내용을 ImageController 등에서만 다 처리한다면?
  * 컨트롤러에서 이미지를 단순히 서버로 받아들이는 기능 외에 너무 많은 기능을 갖게 됨\
    \-> 컨트롤러가 직접 파일을 다루지 않게 하기
* `ImageStorage` -> Infrastructure Service Class로 이해할 수 있음

### 파일 업로드 기능 테스트

* `MockMultipartFile`
  * `~/src/test/resource` 하위 파일을 테스트 실행 시 import하여 모의 파일 업로드 테스트를 가능하게 함
  * MockMvc에 HTTP 요청을 보내는 메소드로써 `mvc.perform(multipart(url))...` 와 같이 post()가 아닌 multipart()를 별도로 사용
* ImageController 테스트 시에는 ImageStorage는 MockBean으로 잡아주기

#### Tips

* 컨트롤러에서는 RuntimeException만 처리해 주어야 하며, 다른 Exception은 가급적 직접 throw 하지 않고 최소한 RuntimeException으로 씌워 주어야 함
