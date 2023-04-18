---
description: 'Task: Custom HTTP Server'
---

# Task: Custom HTTP Server

Low-level Server를 Java로 만들기 (ServerSocket, Socket만 사용)

{% embed url="https://github.com/megaptera-kr/backend-survival-week01/pull/29/commits" %}

* 진행 기간 : 2023. 4. 15. \~ 2023. 4. 15.
* 주요 조건
  * Request header에서 직접 HTTP method와 path를 추출
  * Context path에 맞게 기능 구현
    * /tasks
      * GET (200)
      * POST (201, 400)
    * /tasks/{id}
      * PATCH (200, 400, 404)
      * DELETE (200, 404)
  * 상황에 따른 error handling (400, 404, ...)
  * POST는 201(Created)로 성공 신호 전달
* 새롭게 배운 것들
  * IDE 설치  초기 encoding 설정 챙겨주기
    * UTF-8로 IntelliJ 설정 바꾸어야 함 (사용자 지정 VM 옵션 편집)
    * DELETE method는 response body가 없음 (넣어도 안 보내짐)
    * npm (노드 패키지 매니저/_Node Package Manager_)
      * npm의 tests app을 작동하여 node.js로 작성된 테스트 코드를 돌렸음\
        (node.js를 쓰면 FE-BE의 언어를 js로 통일 가능)
    * Header만 보내도 맨 아래에 빈 줄 (blank line) 빼기
    * Writer는 얼마나 자주 flush해야하는가?
      * 너무 자주도 X, 캐시 메모리 꽉 찰 정도도 X
* 정답 코드 리뷰 소감
  * task id도 method를 호출하여 생성하였음
  * Request 받기, Response 보내기 같이 내가 run에 바로 적은 라인들도 다 method로 구현하는 것이 깔끔.
