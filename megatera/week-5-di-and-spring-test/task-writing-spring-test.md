---
description: 'Task: Writing Spring Test'
---

# Task: Writing Spring Test

{% embed url="https://github.com/megaptera-kr/backend-survival-week05/pull/15" %}

### 새로 배운 점

Unit Test - Integrate Test 등을 기능 뿐 아니라 다양한 부분을 검증하기 위해 사용할 수 있었음

* 그냥 기능만 되게 코딩하다 보면 테스트 과정에서 Bean Creation 과정 에러 또는 NullPointException이 무수히 발생할 수 있음을 깨달았음 -> 테스트 코드 작성을 통해 자연스럽게 코드 리팩토링에 신경쓰게 됨
* given(), verify()와 같은 메소드를 활용하여 기능 작동을 뚜렷하게 파악할 수 있음을 깨달음
* 테스트 코드 작성을 통해서 단순 코드 구조 변경 뿐 아니라 구현 상 오류를 찾아내는 경우도 있었음
* 이번 회차 코드를 자주 찾아볼 것 같음

### 어려웠던 점

* 같은 것을 여러번 구현하고 있어서 이번에는 금방 끝나겠지 하였으나, 기본적인 부분을 잊고서 급하게 진행하다 보면 오히려 작업이 지연될 수 있음을 느낌
* 모든 DTO에는 Getter를 빠트리지 말고 넣어주자... CommentDto의 Getter를 빼먹었는데 에러가 CommentCreateDto bean creation에서 나서 에러를 찾는데 한참 걸렸음
* 이번주는 시간적 여유가 부족하여 엄밀한 의미에서의 TDD를 구현하지는 못하였으나, 테스트 코드를 작성하는 법에 대해서 배워볼 기회가 많지 않기 때문에 큰 도움이 되었음
* Create, Update, Delete 관련 테스트 작성이 특히 까다로웠던 것 같음. 자꾸 Post, Comment가 null이라고 뜨는 것을 회피하는 과정이 아직은 숙련도가 낮아 골치가 아팠음.
