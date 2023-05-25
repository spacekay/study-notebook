---
description: Day 3 Embeddable
---

# Day 3 Embeddable

### @Embeddable

* Aggregate Mapping을 지원 (하나의 객체가 다른 객체의 부품이 됨)
* OOP에서는 primitive type 대신 Value Object로 객체의 field를 구현하는 것을 권함
  * Value Object (VO) : 식별자(Id) 없이 값만 보유하고 있는 객체\
    (ex: 이름, 성별, 나이 등...)
  * JPA에서는 VO를 `@Embeddable`과 `@Embedded` annotation으로 구현 가능
* 사용하는 이유
  * ex> 사람의 이름에 대한 책임은 Name이라는 객체에게, 나이에 대한 책임은 Age 객체에게 위임하면 단일 책임 원칙을 준수하는 데에 유리함

### 구현 예시

```java
@Embeddable
public class Gender {
    // 여기에서 미리 column명 지정 가능
    @Column("gender")
    private String value;
    
    private Gender(String value) {
        this.value = value;
    } 
    
    public Gender female() {
        return new Gender("여");
    }
    
    public Gender male() {
          return new Gender("남");
    }
    // 객체 새로 만들 때 toString(또는 Getter, Setter), equals & hashCode 추가해주기
}


@Entity
public class Person {

    @Id
    @Column("name")
    private String name;
    
    @Embedded // @Column 대신 사용
    // 원래 gender 라고 지정된 컬럼명을 이 테이블에서는 person_gender로 사용함
    @AttibuteOverride(name = "gender", column = @Column(name = "person_gender"))
    private Gender gender;

}
```
