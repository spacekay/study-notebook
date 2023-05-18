---
description: Day 2 Relational Model
---

# Day 2 Relational Model

### Data Model Stages

* [http://agiledata.org/essays/dataModeling101.html](http://agiledata.org/essays/dataModeling101.html)
* [https://www.visual-paradigm.com/support/documents/vpuserguide/3563/3564/85378\_conceptual,l.html](https://www.visual-paradigm.com/support/documents/vpuserguide/3563/3564/85378\_conceptual,l.html)

1. Conceptual Data Model
   * 비즈니스 관점에서 도메인을 고려하여 기능별 데이터 흐름 요구사항을 작성
2. **Logical Data Model** <- 개발자가 주로 구성
   * Conceptual Model을 보고 Database에 구현하기 위해 세부 모델을 택하여 논리화하는 과정
   * 데이터 모델 종류
     * Relational : RDB 등을 통해 가장 자주 접하게 되는 데이터모델
     * Hierarchical : 계층구조를 활용하는 데이터 모델로, Tree 등을 활용하여표현 가능할듯
     * Network : 모든 레코드가 복수의 부모 레코드, 자식 레코드를 가질 수 있도록 하는 데이터 모델. 넓은 의미의 Graph를 통해 표현할듯
       * [https://www.techopedia.com/definition/20971/network-database](https://www.techopedia.com/definition/20971/network-database)
     * Object-Based : ORM을 쓰다 보면 알게 모르게 경험하게 됨
3. Physical Data Model
   * Logical Data Model을 검토하여 실제 Database로 이식하기 위한 DDL을 작성하기 위한 실질적인 DB 구성 진행
     * n:m relationship이 있는 경우 사이에 테이블을 하나 더 만들어 주는 등의 작업을 진행

### Relational Model (관계형 모델)

| 개념            | 정의                                                     | DBMS에서의 구현  |
| ------------- | ------------------------------------------------------ | ----------- |
| Attibute (속성) | <p>(이름, 타입) 으로 구성<br>-> 속성의 이름은 속하는 집합(튜플) 내에서 유일함</p> | Column      |
| Tuple (튜플)    | <p>(속성, 값) 쌍의 집합<br>-> 하나의 튜플 안에는 속성이 중복일 수 없음</p>     | Row, Record |
| Relation (관계) | (속성, 튜플) 쌍의 집합                                         | Table       |

```
튜플 = (속성, 값) 
관계 = 속성(Column) X 튜플(Row, Record)
```

* Relation Model은 Tuple의 집합인 Relation을 통해 DB를 구성
* Relation과 Relationship은 다른 개념임!
  * Relation은 튜플의 집합을 나타내는 표현이고, Relationship은 Entity간 관계를 정의하는 표현
  * Entity-Relationship Model을 통해 Entity간 Relationship을 Relation Model 하에서 정의 가능
* 원래 '집합(Set)'이라고 하면 원소간 중복과 순서가 없어야 하지만, RDB에서는...
  * 서로 다른 Record의 값이 같을 수 있음\
    (단, unique constraint가 있는 column의 값은 같은 Table내에서 중복 불가)
  * 복수의NULL value를 허용함
  * Column간 순서는 DDL 등을 통해 정의했던 순서대로 보통 구성됨

### Relation Variable (관계 변수)

* 관계(테이블)는 시간에 따라 변화므로, 변수로 정의
  * 테이블 이름 자체로 표시

```json
r1 = 
{
    // Heading
    { id/bitint(20), name/varchar(255) },
    
    // Body
    {
        { (id/bigint(20), 1), (name/varchar(255), '종희') },
        { (id/bigint(20), 2), (name/varchar(255), '케이') }
    }
}
```

### Schema

* Relation을 이루는 Attribute의 집합
* 데이터의 구조와 그 표현법, 자료간의 관계를 형식 언어(DDL)로 정의한 것
* Database 전체 또는 일부의 논리적인 구조를 표현함
  * DBMS는 자신이 생성한 Database Schema를 참조하여 명령 수행
