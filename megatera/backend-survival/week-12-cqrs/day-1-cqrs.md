---
description: Day 1 CQRS
---

# Day 1 CQRS

### Command Query Separation

* Greg Young이 제창한 개념
  * [https://cqrs.files.wordpress.com/2010/11/cqrs\_documents.pdf](https://cqrs.files.wordpress.com/2010/11/cqrs\_documents.pdf)
* 객체는 일반적으로 변화하는 **상태**를 가지며, 그 상태를 변화시킬 수 있는 방법은 오로지 **메소드** 뿐
  * 상태를 변경시키지 않는 메소드 (대신 return이 있음) : Query
  * 상태를 변경시키는 메소드 (대신 return이 없음) : Command
  * 예외 : Stack의 pop (객체의 상태도 변경시키고 return도 있음)
    * 적절하게 제약을 걸면 CRUD를 모두 개별적으로 실행시킬 수 있음
* 도메인 지식과 비즈니스 로직은 Command에 집중됨

### CQRS

<figure><img src="../../.gitbook/assets/화면 캡처 2023-06-28 222153.jpg" alt=""><figcaption><p>Source : <a href="https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf">https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf</a></p></figcaption></figure>

* Command Model(Write Model)과 Query Model(Read Model)을 분리하여 서비스를 구성하는 방식
* DB는 하나지만, 두개의 모델로 나누어서 관리
  * Command Model (Write Model)
    * DDD 등을 통해 구현하는 application layer, remote facade 등을 통해 기능 작동
    * 상태를 변화시킬 수 있음
    * 가급적 하나의 서비스에서 한 가지 방식으로만 구현
      * Single Source of Truth (SSOT - 단일 진실 공급원)
  * Query Model (Read Model)
    * 작업이 끝나고난 뒤 현재 상태만 조회하면 됨
    * DTO Fetcher 등을 통해 클라이언트와 DTO를 주고받으며 데이터를 조회할 수 있게 함
    * 상황에 맞게 필요한 기술 적용 (Redis 등)
* 단, Write Model과 Read Model의 내용은 동기화가 되어 있어야 함\
  (상태가 변화되고 나면 그 결과를 Read Model에서도 올바르게 담고 있어야 함)

#### 구현 방식

* **Materialized View**
  * 일부 RDBMS에서 직접 Query 목적으로 사용할 수 있는 View를 제공함
    * 물리적으로 DB에 저장되고, 주기적으로 동기화가 이루어짐
    * Query를 위해 테이블을 만들지 않아도 사용 가능
  * 복잡한 인프라 구성 없이 CQRS 도입하고자 할 경우 사용
* **OLTP** (Online Transaction Processing)
  * 현재 상태를 나타내는 데이터를 사용자에게 보여주는 것 (일반적인 서비스)
* **OLAP** (Online Analytical Processing)
  * Relational Model의 창시자인 에드거 F. 커드가 제안
  * 사용자가 다차원 정보에 직접 접근하여 대화 형태로 다양하게 정보를 분석하고 의사결정에 활용할 수 있게 함
  * Data를 클라이언트에게 서비스하는 관점의 차이
* Event Sourcing
  * 애플리케이션의 상태 변화 -> Event
  * 일련의 이벤트들을 모두 저장함
  * 이벤트 자체는 과거형으로 표현하고, 과거의 이력을 모두 모아서 현재의 상태를 계산할 수 있게 함
  * 단, CQRS와 함께 사용하지 않으면 빠르게 현재 상태를 확인하는 것이 어려울 수 있음
    * 현재 상태는 Read Model에 탑재해 두고, 데이터 활용이나 분석이 필요한 경우는 Write Model에서 Event Sourcing 데이터들을 활용
* 마이크로서비스 구현 시 보다 효과적인 기능 제공을 위해 CQRS를 많이 활용

#### Tips

* Remote Facade
  * 서브시스템을 통해 클라이언트의 요청을 받을 수 있게 구성하는 서브시스템의 interface
  * 서브시스템을 변경해도 클라이언트 코드에는 영향이 가지 않게 구성할 수도 있음
  * Spring MVC에서는 Controller의 형태로 구성
  * 참고
    * [https://live-everyday.tistory.com/210](https://live-everyday.tistory.com/210)
    * [https://martinfowler.com/eaaCatalog/remoteFacade.html](https://martinfowler.com/eaaCatalog/remoteFacade.html)
* Application layer에 Service layer가 포함됨
* 요즘은 게시물 모델 구성 시 내용(조회시 변화 X)과 조회수(조회시 변화 O) 분리하는 경우가 많음\
  → Query - Command 분리
