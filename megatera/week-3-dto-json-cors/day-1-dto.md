---
description: Day 1 DTO
---

# Day 1 DTO

### DTO

* Data Transfer Object
* Tier(주로 F/E - B/E) 간 소통을 위해서는 데이터를 운반하는 수단이 필요
* 오로지 데이터를 담기 위해 쓰이는 **데이터 덩어리**
* Java에서는 어쩔 수 없이 전통적으로 Class로 표현해 왔으나, 최근에는 Record라는 형태로 표현 가능\
  (C/C++의 struct-구조체-와 유사한 개념)
* 특징
  * JavaBeans에 가까움
    * 직렬화되어 있음 (Serializable Interface를 implements) <- DTO로서는 필수적인 건 아님
      * 클래스의 상태를 지속적으로 저장하고 복원할 수 있음
    * **기본 생성자**가 있음 <- DTO로서 기본
    * **Setter, Getter**가 있거나 표준 명명법을 따르는 메소드로 접근 가능함 <- DTO로서 기본
    * (그 외에도 JavaBeans는 필요한 이벤트 처리 메소드를 가지기도 하나, DTO로서의 조건과는 관계 적음)
    * \+ Spring Bean -> Spring IoC Container Beans (JavaBeans가 좀 더 원형이자 근본)
    * POJO (Plain Old Java Object)라고도 표현하며, 결국 Java 프로그래밍 경량화의 핵심이 됨
      * 이것저것(온전한 객체의 조건, 프레임워크 구현 조건 등) 다 갖춘 객체로만 개발하면 오히려 애플리케이션이 무거워짐
  * 제대로 된 객체가 아님
    * 객체는 서로 협력한다 등.. 기본적인 OOP 적용 원칙을 충족시키지는 않음

### IPC

* Inter-process Communication
* 서로 다른 프로세스(또는프로그램) 간 통신을 의미
* Tier(물리적인 구분)를 나누게 되면 그 사이 IPC는 필수
  * File은 원격 통신 불가
  * Socket은 File과 비슷하지만 원격 통신이 가능함
    * HTTP 같은 고수준 프로토콜을 사용하면 편리함
* XML, JSON과 같은 직렬화 기술을 활용하여 서로간에 소통함
* RPC (Remote Procedure  Call)
  * 원격 프로시저 호출이라고 하며, 원격 제어를 위한 코딩 없이 상대방의 장치에서 알아서 함수나 프로시저를 실행할 수 있게 함\
    (예: 서버에서는 클라이언트 장치를 동작하려고 하지 않아도 됨.\
    클라이언트 컴퓨터에게 이렇게 동작하라는 Call을 보내는 것으로 충분함)
  * 객체지향 프로그래밍에 적용 시 RMI(Remote Method Invocaion, 자바 원격 함수 호출)라는 용어로도 표현함
  * REST 적용 시 resource를 기준으로 설계하여야 하여 단순 RPC보다 제약조건이 많아질 수 있음\
    (상대방이 이해할 수 있게 짜는 것 뿐 아니라 올바른 리소스로 잘 요청할 수 있는지도 고려하여야 함)

### Tier 간 통신

* F/E - B/E
  * DTO를 그대로 전송할 수는 없고, **직렬화**(마샬링)하여야 함
  * **String** 또는 **Byte String**으로 전송하기 위해 DTO 구조를 XML, JSON(단순, 이해하기 편함) 형태로  변환
* B/E - DB
  * **DDD (Domain Driven Design)** <- 추후에 직접 개발하며 느낄 수 있는 기회가 있으면 좋겠음
    * [https://martinfowler.com/bliki/DomainDrivenDesign.html](https://martinfowler.com/bliki/DomainDrivenDesign.html)
    * [https://huisam.tistory.com/entry/DDD](https://huisam.tistory.com/entry/DDD)
    * [https://velog.io/@dnflekf2748/DDDDomain-Driven-Design](https://velog.io/@dnflekf2748/DDDDomain-Driven-Design)
    * 도메인 : 실세계에서 사건이 발생하는 집합
      * 주문 도메인(고객), 매장 관리 도메인(점주), 건물 관리 도메인(건물주)&#x20;
      * 같은 객체여도 도메인별로 다른 정보를 가질 수 있음 -> Context에 따라 객체의 역할이 바뀜 -> Bounded Context
      * 각각의 도메인을 대표하는 객체(Aggregate)를 통해 Infra(DB 등)와 소통 -> Repository를 Aggregate 단위로만 만듦&#x20;
        * Aggregate에 해당 도메인 부속 객체들이나 클래스들이 모두 적절하게 엮여 있음
    * 이벤트별 액터를 가정하고, 비즈니스 도메인별로 소프트웨어를 설계함
    * 확장성을 고려하고 **모듈간의 의존성은 최소화하고 응집성은 최대화**함
    * 이 과정에서 Context mapping을 진행할 수 있고, context별로 서비스를 관리
  * JPA, ORM을 사용하는 것은 DDD 구현에는 바람직하지 않을 수 있음 -> 그래도 Java + JPA는 강력함
  * Kotlin 사용 시에는 Exposed와 결합하여 Active record + DTO 로 ORM의 역할을 분리하여 사용하는 방향으로 가고 있음
