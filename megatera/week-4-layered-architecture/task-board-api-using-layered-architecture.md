---
description: 'Task: Board API using Layered Architecture'
---

# Task: Board API using Layered Architecture

{% embed url="https://github.com/megaptera-kr/backend-survival-week04/pull/12" %}

#### Ground Rules

* 3주차와 구현하는 기능 자체는 유사하지만, 3주차 코드를 복붙하여 구현하지는 말자.
* 다양한 DTO를 만들어서 기능에 맞게 사용하자.

#### 새로 배운 점

* stream().map()를 쓸 때 발생할 수 있는 여러 예외상황을 경험하였음.\
  (생성자에 parameter 안 넣어서 필드 값이 다 null로 나오기, stream을 돌릴 객체 자체가 없음 등)
* Controller부터 Repository까지도 코딩해보고, 그 반대 방향으로도 시도하면서 가급적 Controller부터 해야겠다는 생각을 하게 됨.
* npm codeceptjs 를 돌리려면 먼저 front-end 서버를 한번 켜서 빌드를 해야 함(dist 폴더 생겨야  함)을 이번(4주차)에 깨달음

#### 어려웠던 점

* Comment는 Post와 연관되어 있는 entity이다 보니 어떤 방식으로 저장용 객체를 구현할지에 대한 고민이 컸음.\
  (HashMap 안에 List를 넣으려니 번거로워서 그냥 이중 HashMap으로 구현하였는데 실성능이 어떨지는 모르겠음)
* 이미 한 번 3주차에서 해본 것인데도 Layered Architecture를 적용하여 다시 짜는 것은 어렵다는 것을 체득함.
