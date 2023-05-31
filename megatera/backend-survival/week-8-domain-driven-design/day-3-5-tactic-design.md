---
description: Day 3 ~ 5 Tactic Design
---

# Day 3 \~ 5 Tactic Design

### DDD Lite : Model-Driven Design

* 도메인 모델으로 사용하는 대표적인 패턴
  1. Entity
  2. Value Object (VO)
  3. Aggregate
  4. Repository

#### Entity

* 대부분의 객체는 속성이 아닌 **연속성**과 **식별성**에 따라 정의

```
앨리스는 덩치가 커져도, 작아져도, 앨리스임을 알 수 있음 -> 덩치 : 속성
```

* 식별자(Identifier)가 존재하고, 이를 통해 동일성(Identity)을 확인할 수 있다면 DDD에서의 Entity라 할 수 있음

#### Value Obejct (VO)

* 어떤 객체는 연속성과 식별성이 중요하지 않음
* 속성만을 통해 **동등성**(Equality)을 판단함
  * 가능하면 field의 값이 변하지 않도록 불변 객체(final field)로 생성
  * Java Class로 작성 시 equals() 및 hashCode() 구현 필수
* 무엇이 Entity이고, VO인지는 비즈니스 도메인에 달려 있음
* Java 14부터 제공하는 Record를 활용하면 쉽게 구현할 수 있지만, JPA와의 호환성이 아직 낮음
  * [https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Record.html](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Record.html)
  * Hibernate 6.2부터 Record를 지원하는데, Spring Boot 3.1부터 이를 지원한다고 함

#### Aggregate (집합체)

* 불변식(invariant)을 유지하고 도메인 객체를 사용하기 좋게 만들어 줌
  * 불변식 : 제약 조건, 규칙 -> Aggregate 내부 일관성을 파괴하는 행위를 방지하여 무결성을 유지함
* 특정 Aggregate를 사용하기 위해서는 반드시 aggregate root 객체를 통해 접근해야 하며, 내부의 객체에는 개별적으로 접근할 수 없음\
  \-> 이를 Transactional 설정 등을 통해 구현함 -> 불변식 준수
* Aggregate의 동작 자체에 집중하여 application을 구현하도록 노력해야 함

<pre><code>&#x3C; Example >
<strong>* Entity : Order, Item
</strong><strong>* 불변식 : 주문별 총 금액은 100만원 이하여야 함
</strong>
[Order Aggregate] Order (Aggregate Root) - listItem (1)
                                         - listItem (2)
                                         
order.addItem(new Item("1번 아이템")); &#x3C;- '주문에 아이템을 추가한다'라는 동작에 집중
</code></pre>

* 협력하는 객체를 구현하기 위하여 책임, 위임, 관심사의 분리와 같은 주제들을 통해 고민하고 적용함\
  (단일 책임 원칙 적용 예시 : 하위 객체 `Item` 처리 기능을 상위 객체 `Order`에게 위임
* 올바른 Aggregate 설정 방법
  1. 불변식을 통해 일관성 관계를 찾아 모델링 (Aggregate = Transactional 일관성 경계)
  2. 작은 Aggregate로 설정
  3. Id로만 다른 Aggregate를 참조함\
     (JPA에서는 여러 annotation으로 다른 객체를 참조할 수 있지만 가급적 Id만 활용하는 것이 안전함)
  4. Aggregate 경계 밖에서는 결과적 일관성을 유지함\
     (작업 시 내부에서 객체들간에 어떻게 작용하건 외부에서 보았을 때 정상적으로 작동하여 의도한 결과가 잘 반영되면 됨)

#### Repository

* Aggregate를 관리하는 Collection Interface처럼 작동
* 주요 원칙
  1. 오직 Aggregate만 Repository를 가짐 -> 이 문서의 예시 기준으로 `OrderRepository`만 존재함
  2. Repository는 영속화 방법 및 기술을 감춤 -> Interface이므로 가능함\
     개발자가 해야할 것은 CascadeType.ALL, orphanRemoval=true 등을 적용하여 객체간 관계를 구성하는 것
  3. Repository는 사실상 전역으로 접근 가능 (프로젝트 내부 어떤 레이어에서든 접근 가능)
  4. Spring에서는 DI를 활용해 Repository 객체를 Singleton으로 관리함

### Tactical Design

1. Aggregate 발견
2. 적절히 책임을 나눌 수 있도록 Aggregate 내부를 Entity와 Value Object로 구성 (또는 분해)
3. 비즈니스 도메인에 집중한 코드를 모은 곳 -> Domain Layer
4. Repository Aggregate를 사용하는 코드가 모인 곳 -> Application Layer (App의 기능, Use case가 드러남)
5. Web 등 구체적 기술로 사용자와 소통하는 코드가 모인 곳 -> UI Layer

\=> Layered Architecture 없이는 도메인 모델을 Domain Layer에 격리할 수 없으므로, Layered Architecture가 DDD 구현의 핵심이 됨
