---
description: Day 3 Cloudinary (Image/Video cloud service)
---

# Day 3 Cloudinary

### Cloudinary

* [https://cloudinary.com/](https://cloudinary.com/)
* 강력한 이미지/비디오 관리 기능을 가진 Cloud service
* 썸네일 이미지 즉각 생성 및 캐시 기능
  * [https://cloudinary.com/documentation/image\_transformations](https://cloudinary.com/documentation/image\_transformations)
  * Cloud에 저장된 이미지 url에 parameter와 그의 값을 추가하기만 해도 해당 이미지의 크기 조정, 블러 처리 등 효과 추가 가능
  * 캐시 기능을 통해, 같은 이미지로 유사한 작업을 여러번 할 경우, 아예 새로운 작업을 할 때보다 훨씬 빠르게 작동
* Spring에서 사용할 때는 `build.gradle`에서 dependency 설정 후, yml에 개인 키 등을 넣어준 후 사용 가능 (API 연결 정보는 서버 실행 시 -Edit Configuration 등을 통해- 환경 변수로 넣을수도 있음)
