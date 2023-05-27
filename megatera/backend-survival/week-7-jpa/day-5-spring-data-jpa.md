---
description: Day 5 Spring Data JPA
---

# Day 5 Spring Data JPA

### Dependencies

```gradle
implementation 'org.springframework.data:spring-data-jpa:3.0.6' // 2023.5.28. 기준
implementation 'DB Driver'
```

### DAO vs. Repository

* [http://aeternum.egloos.com/1160846](http://aeternum.egloos.com/1160846)
* DAO
  * Data access가 중점
* Repository
  * 객체를 관리하는 Collection interface 같은 느낌

### Spring Data JPA

* 기본적으로 JpaRepository interface를 implement할 때, \<Entity, Entity id의 타입>으로 제네릭 타입을 지정
* find, save, delete, exist(s) 등 여러 동사에 \~All, \~By(Column명) 등을 붙여서 해당 메소드를 구현하지 않아도 기본 설정대로 SQL을 생성함

<table><thead><tr><th width="261.3333333333333">메소드명</th><th width="178">HTTP</th><th>SQL</th></tr></thead><tbody><tr><td><code>findAll()</code></td><td>GET</td><td>SELECT * FROM table;</td></tr><tr><td><code>findById()</code></td><td>GET</td><td>SELECT * FROM table WHERE id = ?;</td></tr><tr><td><code>save(Entity entity)</code></td><td>POST, PUT, PATCH</td><td>INSERT, UPDATE</td></tr><tr><td><code>delete(Entity entity)</code></td><td>DELETE</td><td>DELETE</td></tr></tbody></table>

### Transaction 관리

* `@Transactional`
  * Spring AOP를 활용하여, 클래스 또는 메소드의 실행 전후 시점을 기준으로 트랜잭션을 관리
  * 일반적으로는 Application layer가 트랜잭션 범위가 되므로, Service 클래스 상단이나 create, update, delete 관련 service 메소드들 상단에 달아주곤 함
    * 도메인-인프라스트럭처 레이어와 소통하는 영역이므로 Application layer까지만 트랜잭션을 관리하고, 그 위 레이어로는 연관되지 않도록 처리

#### Tips

* existsById()로 해당 테이블의 row 유무 확인이 가능하므로, 그냥 데이터가 있는지 확인만 할 경우에는 사용하는 것도 괜찮을듯.
* Service Test 시 Repository는 Mocking하여 사용하고, `@Transaction` annotation을 달아두면 테스트가 끝난 후 자동으로 해당 트랜잭션을 rollback함 -> 부담없는 테스트 가능
