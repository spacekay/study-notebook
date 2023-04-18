---
description: Day 4 Collection Pattern 적용
---

# Day 4 Collection Pattern 적용

### CRUD

| Name   | HTTP methods | Classification     | Details                                      |
| ------ | ------------ | ------------------ | -------------------------------------------- |
| Read   | GET          | Query (Builder)    | <p>상태가 변하지 않음. 안전함.<br>분산 처리, 캐시 작업 수월함.</p> |
| Create | POST         | Command (Modifier) | 상태가 변함. 안전하지 않음.                             |
| Update | PUT, PATCH   | Command (Modifier) | 상동                                           |
| Delete | DELETE       | Command (Modifier) | 상동                                           |

* Query의 장점
  * 요청할 때마다 항상 같은 결과를 얻을 수 있음
    * 여러 환경에서 분산 처리 가능
    * 요청 결과 캐싱해 두고 나중에 꺼내쓰기 가능

### Collection Pattern with HTTP methods

* 같은 패턴(리소스)으로 다른 메소드를 보내면 다른 작업을 함
* 게시물(posts) 케이스

| HTTP method | Collection Pattern | Operation                       |
| ----------- | ------------------ | ------------------------------- |
| GET         | /posts             | Read Collection (List)          |
| GET         | /posts/{post\_id}  | Read Element (Detail)           |
| POST        | /posts             | Create                          |
| PUT, PATCH  | /posts/{post\_id}  | Update Element                  |
| DELETE      | /posts/{post\_id}  | Delete Element                  |
| PUT, PATCH  | /posts             | Update Collection (Bulk update) |
| DELETE      | /poste             | Delete Collection (Bulk delete) |

* 가급적 Bulk 작업은 하나하나 하기보다는 별도 API 정의해서 진행하자.



* 댓글(comments) 케이스 -> GET, POST만 정리

| HTTP method | Collection Pattern                                              | Operation           |
| ----------- | --------------------------------------------------------------- | ------------------- |
| GET         | /comments                                                       | 전체 댓글 목록 조회         |
| GET         | /comments/{id}                                                  | 특정 댓글 상세 조회         |
| GET         | /comments?post\_id={post\_id}                                   | 특정 게시물에 대한 댓글 목록 조회 |
| POST        | /comments                                                       | 댓글 생성               |
| POST        | <p>/comments?post_id={post_id}<br>/posts/{post_id}/comments</p> | 특정 게시물에 대한 댓글 생성    |

* 로그인 / 로그아웃 (추상적인 개념 적용)

| HTTP method | Collection Pattern                         | Operation                                                                                                                                     |
| ----------- | ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| POST        | /session                                   | 세션 생성 (로그인)                                                                                                                                   |
| DELETE      | /session                                   | 세션 파괴 (로그아웃)                                                                                                                                  |
| GET         | /session                                   | 내 정보 보기                                                                                                                                       |
| GET         | <p>/user/me<br>( 원래 /user/{user_id}  )</p> | <p>내 정보 보기<br>(단, user_id="me"인 경우에 내 정보 보기로 처리한다는 사실을 API Docs에 반드시 기록하여야 함<br>-> 접속 중인 유저 아이디는 <strong>Header</strong>를 통해 확인하는 경우가 많음)</p> |

