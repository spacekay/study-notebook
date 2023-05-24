---
description: Day 2 Hibernate
---

# Day 2 Hibernate

#### Introduction

```gradle
implementation 'org.hibernate:hibernate-core:6.2.3.Final' // 2023. 5. 25. 기준
implementation 'DB Driver'
```

### Entity Object

* 도메인 모델을 Entity object로 구현함
  * 주요 Annotation
    * @Entity : Entity object임을 명시함 (**필수**)
    * @Table : Entity class 이름과 DB 상 테이블 이름을 매핑
    * @Column : Entity class에서의 field 이름과 테이블 상 column 이름을 매핑
* Entity object model과 data model 사이의 불일치 문제에 대한 추가 설명
  * [https://en.wikibooks.org/wiki/Java\_Persistence/Mapping](https://en.wikibooks.org/wiki/Java\_Persistence/Mapping)

```java
// Entity class 예시 코드
```

### JPA 구현

* 진행 방법 (테스트 코드 작성 과정 위주)
  1. `EntityManagerFactory`는 `EntityManager`를 생성해 주는 역할을 하며, Factory가 생성 시 시간이 훨씬 오래 걸림\
     따라서 Factory는 한 번만 생성하고, Manager는 테스트 실행 시 생성 후 끝나면 close해 줌\
     (DB의 안정성 유지를 위해 필수. 프로그램 실행 중일 때만 DB와 연결이 되어 있어야 함)
  2. @BeforeAll 로 `EntityManagerFactory` 객체 생성하는 메소드를 static으로 작성
  3. @BeforeEach 로 `EntityManager` 객체 생성하는 메소드를 static으로 작성
  4. @AfterEach 로 `EntityManager` 객체를 닫는 메소드를 static으로 작성
  5. `/resource/META-INF/persistence.xml`을 생성하여 DB 연결 정보 추가\
     (Spring에서는 application.yml 등으로 DB 연결 정보를 작성하면 알아서 설정해 줌)
     * Persistence unit명은 Factory 생성 시 parameter로 사용
  6. 이후 실제 테스트 메소드를 @Test 로 작성
     1. Transaction 정의를 하지 않으면 CRUD 후 update 내용이 반영되지 않음
        * EntityTransaction object를 생성한 뒤, `transaction.begin()` 후 실제 CRUD 코드 작성
        * 작업 완료 시 `transaction.commit()`하여 변경 내용을 commit함

#### EntityManager를 통한 SQL query 구현

<table><thead><tr><th width="178">SQL (row 기준)</th><th>EntityManager method</th></tr></thead><tbody><tr><td>SELECT</td><td>.find(Entity.class, "id값");</td></tr><tr><td>INSERT</td><td>.persist(Entity);</td></tr><tr><td>DELETE</td><td>.remove(Entity);</td></tr><tr><td>UPDATE</td><td>Entity.changeValue(~);<br>-> Entity object의 field 값 자체를 변경 후 commit하면 DB 상 해당 row data에도 반영. 별도 .update(); 같은 메소드는 없음.</td></tr></tbody></table>

#### JPQL

* Jakarta Persistence Query Language
* 실제 SQL query를 작성하여 EntityManager로 실행 가능
* EntityManager의 자체 메소드만으로는 얻을 수 없었던 여러 Entity 정보를 한번에 결과값으로 받을 수도 있음\
  (ex: List\<Entity>,  Page\<Entity> 등)

#### 구현 예시

```java
// Jpa test code 시
```
