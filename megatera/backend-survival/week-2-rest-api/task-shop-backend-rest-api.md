---
description: 'Task: Shop Backend REST API'
---

# Task: Shop Backend REST API

다른 것 없이 주어진 기능을 REST API로 설계하여 간략하게 구현하는 연습

{% embed url="https://github.com/megaptera-kr/backend-survival-week02/pull/20" %}

#### 주의사항

* Gradle Build 시 JDK 버전 체크\
  \-> 두군데 다 스프링 버전(3.0 -> Java 17 이상)에 맞게 설정하기
  * Project Structure
  * Settings - Build \~ - Build Tools - Gradle

#### 주요 구현 내용

* REST API를 조건에 맞게 설계하기
* Resource를 URI로 표현하기
* Request에서 필요한 내용 찾기
* 적절한 Response body 작성하기

#### 어렵거나 아쉬웠던 점

* 테스트 코드를 짜보려니 DTO 등.. 현재 주어진 조건보다 많은 부분을 구성해야 하는 점에 부담이 있었음
* Error Handler까지 구현을 하다 보면 이번 주차 목표 달성보다 Configuration 작업이 더 오래 걸릴 것 같아 일단 커리큘럼을 따라가는 것을 우선으로 하였음
* 과제 Repository를 fork해온 후 자꾸 branch 신규 생성하는 것을 까먹어서, 다음주부터는 git 관련 설정부터 확실히 하고 개발을 시작할 예정

#### 새롭게 알게 된 내용

* Resource를 어떻게 URL로 표현하여야 할 것인가에 대해 집중하여 생각할 수 있었음
* @RequestAttribute annotation을 devroad 시작 이후 처음으로 사용하게 되었음
* 추후 신규 API 개발 시 Custom error handler를 상세하게 설계해 보아도 좋겠다고 생각하였음

#### 해설과의 차이점

* user -> users
  * user도 복수로 쓰기
* 장바구니에 상품 추가 `POST /cart/items`
* 장바구니에 상품 삭제 `DELETE /cart/items/{id}`
* 장바구니 (담긴 상품 목록) `GET /cart`
  * cart를 맨 앞에 적어주는 것이 맞았다.
  * products -> items (상품과 카트 안의 아이템을 분리하기)
* 주문하기 `POST /orders`
  * orderDTO 만들어서 POST로처리 -> 주문 관련 설정 같은 것이 있을 수도 있으니...
* 주문 목록 `GET /orders`

