---
description: 'Task: Connecting with JPA'
---

# Task: Connecting with JPA

{% embed url="https://github.com/megaptera-kr/backend-survival-week07/pull/12" %}

### 새롭게 배운 점

* [@activeprofiles](https://github.com/activeprofiles)("test")는 소중하다.
* JPA를 사용할 때는 domain model 관련 class에는 모두 빈 생성자(parameter 0인 경우)의 정의가 필요하다. [@entity](https://github.com/entity)건 [@embeddable](https://github.com/embeddable)이건 상관 없다.
* Comment의 postId field를 Post 테이블을 아예 join해 주면서 [@manytoone](https://github.com/manytoone), [@joincolumn](https://github.com/joincolumn) 하여 연결하였다.

### 어려웠던 점

* 예전에 프로젝트를 했었을 때는 [@embeddable](https://github.com/embeddable)을 사용하지 않았어서 이번 과제를 수행하며 다채롭게 단계별 에러를 경험하였다.
* Id 역할을 하는 객체들을 다루는 것이 까다로웠다. @EmbeddedId라는 annotation을 써주니 에러가 자꾸 생겨서, PostId와 CommentId의 id field에 [@column](https://github.com/column)(insertable = false, updatable = false)라는 annotation을 추가하였다. 이렇게 하니 Id 중복 생성을 막을 수 있었다.
* 구조가 바뀔 때마다(DAO -> JPA 등) 테스트 코드도 계속 바꾸어 주어야 하고 생각보다 손이 많이 갔다.
