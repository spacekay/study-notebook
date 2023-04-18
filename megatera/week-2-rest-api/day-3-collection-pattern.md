---
description: Day 3 Collection Pattern
---

# Day 3 Collection Pattern

### Collection Pattern

* 여러 리소스 중 공통점이 있는 것끼리 하나의 그룹으로 묶어줌
  * ex: 게시물(post) -> 하나의 '게시물 그룹(posts)'으로
  * 주로 복수형 사용 (그 중 하나를 지칭할 때도 Pattern에는 복수형으로 기입)
* Resource Id -> URI -> URL
  * PostId -> posts 그룹 내 식별자로서, resourse ID의 구성 요소 중 하나
* 특정 컬렉션의 하위 컬렉션 설정 (ex: comments)
  * /posts/{post\_id}/comments <- comment directory처럼 설정
  * /comments?post\_id={post\_id} <- 쿼리로도 작성 가능함
  * 해당 리소스에 접근하는 방향성은 다르나 결과는 동일함
  * /comments/{comment\_id} <- 원글의 정보를 알 수 없어, 간단한 작업(DELETE)에만 편리하게 사용
* 어차피 REST API에서는 페이지를 다 표현할 필요가 없음... -> 페이지에 표현하는 것은 FE의 업무
* 단수형 사용하는 케이스
  * Session -> Client 당 무조건 1개
* Collection Pattern 구성 시 유의사항
  * 가급적 Resource는 명사로 정의하는 것이 좋음. 하지만 동사로 쓰고 싶다면...
    * 상위 명사를 좀더 범용적으로 만들어서 접근하는 것도 가능
      * ex) /pages/edit-posts 그리고 /pages/edit-comments <- 둘 다 pages 리소스로 들어감
  * 너무 복잡하게 하지 않기
    * collection/item/collection 이상으로는 X <- 웹 서버 부하를 줄이기 위함
