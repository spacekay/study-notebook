---
description: 'Task: Trying to use DDD'
---

# Task: Trying to use DDD

### 새롭게 배운 점

* 객체를 컬럼으로 사용하는 경우에 발생할 수 있는 위험성들을 경험하였다. (컬럼 길이 부족, 의존성 문제, 테이블 생성 오류 등..) 몇 번 더 비슷한 프로젝트를 경험해 보아야 같은 실수를 반복하지 않을 것 같다.
* H2에서 객체를 컬럼으로 받는 경우 해당 컬럼의 사이즈가 충분해야 한다. (255로 부족하여 1000으로 assign하니 통과)
* 테스트 시 Cart 객체도 mocking을 해주니 내부 메소드 등을 호출하는 것이 가능했다.
* CartId 구현 시 final static으로 고정된 ID값을 정의하여도 무관하다.

### 어려웠던 점

* 이번에는 카트가 하나 뿐이어서 그나마 나았지만, 카트도 여러 개고 주문도 받는게 있었으면 힘들었을 것 같다.

{% embed url="https://github.com/megaptera-kr/backend-survival-week08/pull/11" %}
