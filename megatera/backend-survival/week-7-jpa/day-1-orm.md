---
description: Day 1 ORM
---

# Day 1 ORM

### Object-Relational Mapping

* 객체와 관계형 데이터를 매핑하는 작업, 기술, 도구
  * 도구로서의 의미를 강조하는 경우 M을 Mapper로 해석함
* 주요 구현 내용
  * SELECT -> 객체 생성
  * UPDATE -> 객체 상태 활용 (필드값 수정 등) 등 객체 변화 감지
  * TRANSACTION, COMMIT, ROLLBACK -> 객체 생명주기 관리
* 장점
  * SQL query를 자동 생성하고 매핑도 자동으로 해주어, 개발자의 SQL query 작성을 최소화함
    * 다양한 DBMS에 대응하는 것이편리함
    * 비즈니스 로직에 집중할 수 있음
    * 결과적으로 유지보수가 편리해짐
    * 객체지향 원칙을 따르는 도메인 객체를 사용하여 DB 상 데이터와 매핑이 가능해짐

### JPA

* Jakarta Persistance API
  * Java EE가 오라클로 넘어간 후, 오픈소스 진영에서는 Jakarta EE라는 이름으로 바꾸어 부르고 있음\
    (언어 이름 자체는 여전히 Java라고 하지만.. 패키지명 등은 점차 java\~ 등에서  jakarta로 바뀌어 가고 있음)
* Java에서 사용하는 ORM 표준
  * JPA 자체는 interface(spec)만 정의하고 있으므로, 구현체가 필요함
* 주요 구현
  * 공식 참조 구현 : EclipseLink (Open source)
  * 오랫동안가장 널리 쓰이는 것 : Hibernate (기업에서 운영)
    * Hibernate 출신 주요 개발자들이 JPA spec 자체에 많은 영향을 끼쳤으며, 기능도 많음

### JPA Entity

* DB의 데이터와 객체를 매핑하기 위해 ERM에서의 Entity라는 단위를 활용
  * 간단하게 생각하면 Entity class는 해당 테이블의 스키마(attribute 정보 등)를 나타내며, 각각의  Entity object(instance)는 각각 하나의 row에 들어있는 데이터를 담고 있음
* DB - OOP 둘 다 고려하여 설계하여야 JPA의 장점을 적극 활용 가능
* Relationship Mapping, Aggregate Mapping
  * Id 등을 통해 데이터의 중복도 막고 Entity간 관계를 객체를 통해 표현 가능

#### Tips

* Aggregation
  * 해당 객체의 부품 역할을 하는 것으로, 없어도 작동은 함
* Composition
  * 해당 객체의 일부분이 되는 것으로, 없으면 작동 안함
