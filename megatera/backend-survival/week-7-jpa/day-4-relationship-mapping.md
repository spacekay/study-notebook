---
description: Day 4 Relationship Mapping
---

# Day 4 Relationship Mapping

### Relationship Mapping

* JPA에서는 Entity (type) 간의 관계를 객체 참조를 통해 간단하게 구현해줌\
  (JPA 사용 초기에는 밑줄 그은대로 하는 것이 안전)
* DDD Aggregate를 구현하기 위해서는  주로 `@OneToMany` annotation에 `CascadeType.ALL`, `orphanRemoval=true` 설정
  * Person 객체가 Item 객체를 가진 경우, Person이 Item을 소유하고 있으므로 Item에 외부에서 개별적으로 접근해서는 안됨
  * CascadeType
    * [https://zzang9ha.tistory.com/350](https://zzang9ha.tistory.com/350)
    * 영속성(Persistence) 전이란? -> 특정 entity를 영속화하였을 때 연관 entity도 영속 상태로 전이되는 경우를 의미
    * `CascadeType.ALL` : 상위(부모) entity의 상태 변화가 하위(자식) entity에도 그대로 모두 반영
  * orphanRemoval
    * [https://tecoble.techcourse.co.kr/post/2021-08-15-jpa-cascadetype-remove-vs-orphanremoval-true/](https://tecoble.techcourse.co.kr/post/2021-08-15-jpa-cascadetype-remove-vs-orphanremoval-true/)
    * 자식 entity를 업데이트할 경우, 새롭게 만든 자식 entity 객체에 부모 entity가 연결되고 기존 자식 entity 객체는 null로 바뀜\
      \-> 이와 같이 연관관계가 끊기거나, 삭제  커맨드 등으로 인해 부모 entity와의 관계를 잃어버린 객체를 orphan이라 함
    * Orphan entity가 발생하는 경우 삭제할지 설정하는 옵션이 orphanRemoval
    * 어차피 CascadeType.ALL을 적용한 케이스에서는, 특정 부모 entity에 연결되어 있던 자식entity가 단독으로 남아있는 것이 의미가 없므로, `orphanRemoval = true` 옵션을 사용함
    * 단, 자식 entity가 여러 entity에 엮여있다거나.. 한 경우에는 다른 옵션을 써야 할 것 같음
* 단, 너무 많은 객체들끼리 엮어두면 작업 시간이 길어질 수도 있으므로...
  * 처리 시간 단축이 중요한 조회 작업의 경우 JPA Entity를 활용하지 않는 것이 더 빠를 수 있음
  * Command : JPA Entity, Relationship Mapping을 모두 활용
  * Query : SQL Mapper(jdbcTemplate 등)로 바로 작업하는 것 추천
* Item 객체를 현재 Person 객체가 소유하고 있다면, Item 객체 처리를 Person 객체로 위임할 수 있음
  * 이 경우 Person에 대한 DAO만 사용하여도 Item 객체까지 처리할 수 있음
  * 단, items list의 사이즈가 지나치게 커진다거나 해서 Item 객체의 비중이 Person 객체보다 훨씬 커져 주객전도 현상이 발생할 수 있음\
    \-> 이 경우 ManyToOne mapping 등을 통해 다른 방식으로 JPA를 구성하여야 함

{% hint style="info" %}
필요에 따라서는 JPA, RDB 대신 NoSQL을 활용하여 Event Sourcing을 도입할 수도 있음\
(DB 사용 시 현재의 상태만을 확인할 수 있지만, Event Sourcing은 작업 과정에서 발생하는 모든 event를 저장해 둠)
{% endhint %}

### 구현 예시

* Annotations
  * `@OneToMany(CascadeType.ALL, orphanRemoval=true)`
  * `@JoinColumn(name="외래키 이름", referencedColumnName="자식 테이블에서 참조하는 column 이름 (별도 지정 없으면 id)")`
  * `@Orderby("리스트의 정렬 기준이 될 자식 테이블 컬럼명")`
* OneToMany : CascadeType.ALL, orphanRemoval=true로 하여도 괜찮음
* ManyToOne : 다른 설정이 필요할 수도 있음

```java
@Entity
public class Person {

    @Id
    @Column("name")
    private String name;
    
    @OneToMany(CascadeType.ALL, orphanRemoval=true)
    @JoinColumn(name="item_id")
    // referencedColumnName을 별도 정의하지 않으면 자동으로 id로 적용
    @OrderBy("name") // Item 객체에서 정의한 컬럼명 기준으로
    private List<Item> items = new ArrayList<>();

}
```

#### JPA 사용 시 불편했던 점

1. persistence.xml에 Entity class 모두 추가하기
2. EntityManager 객체 얻는 과정이 번거로움
3. Transaction 관리가 어려움

\->  이 모든 것을 **Spring Data JPA**를 통해 해결 가능

