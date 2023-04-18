---
description: Day 1 REST API
---

# Day 1 REST API

### API

* Application Programming <mark style="background-color:purple;">**Interface**</mark>
  * 소프트웨어에서 연산을 하기 위해 소통하는 방식 (결국 Interface임)
  * 구성요소
    * 연산의 이름 (함수 이름)
    * 매개변수 객체 (Parameter)
    * 연산의 반환값 (Return)
    * 연산의 Signature -> 시스템의 무결성과 보안성을 지키기 위한 인증 절차
  * 특징
    * Communication
      * 결국 사람이 쓰는 것이므로 사람이 알아볼 수 있어야 함
    * Specification
      * 개발자들끼리는 Swagger.io나 API Docs 등을 통해 해당 API의 스펙 확인 가능
    * Information Hiding (정보 은닉) -> 개념, 원리
      * 소프트웨어 작동 시 사용객체에 대한 구체적인 정보를 노출시키지 않아야 함 (OOP)
    * Encapsulation -> 실현 방식
      * 정보 은닉을 위해 실제 개발 시 주로채택하는 방식
      * API의 작동 방식만으로는 내부 구동 방식을 알아챌 수 없음

### REST

* Representational State Transfer
* 네트워크 소프트웨어 아키텍처 원리 '의 디자인'\
  \-> 네트워크 소프트웨어 아키텍처를 구현하기 위한 구조나 방식, 스타일을 의미
* 특징
  * 성능, 규모 확장성, 단순성, 수정 용이성, 가시성, 이식성, 신뢰성이 좋다고 함
  * [https://blog.npcode.com/2017/03/02/%EB%B0%94%EC%81%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%93%A4%EC%9D%84-%EC%9C%84%ED%95%9C-rest-%EB%85%BC%EB%AC%B8-%EC%9A%94%EC%95%BD/](https://blog.npcode.com/2017/03/02/%EB%B0%94%EC%81%9C-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%93%A4%EC%9D%84-%EC%9C%84%ED%95%9C-rest-%EB%85%BC%EB%AC%B8-%EC%9A%94%EC%95%BD/)
* REST style 아키텍처 구현 시 제약조건
  * Null Type으로 시작하기 (ex: www.)
  * Stateless
  * Cache
  * Uniform Interface (단순하고 가시적이어야 함)
    * REST는 Interface로서 작용하여, FE-BE 등 서비스간의 분리를 가능하게 함\
      (서비스간 소통의 도구 또는 방식)
    * 서비스별 독립적인 관리가 가능하나, 표준화된 규격이므로 효율성이 저해될 수는 있음
    * Common case of Web <- 웹 개발 시 일반적으로 적용 가능한 스타일
    * 필딩 제약 조건 (Uniform Interface 구현을 위한 조건)
      * Identification of Resources
        * URI 등으로 리소스 식별하기
      * Manipulation of Resources
        * 표현을 통한 리소스 조작 (쓰기, 지우기 등)
      * Self-descriptive Messages
        * 요청하는 내용을 구체적으로 서술하여 받는 쪽(컴퓨터)이 request의 상태-의도를 잘 이해할 수 있도록 함
          * 사람은 Content type의 추론이 가능하지만, 프로그램은 불가능하므로 이를 개발자가 잘 정의해 주어야 RESTful해짐
        * 주로 Content type, Media type 등을 명시하는 식
        * 나만의 custom type 같은 것을 쓰면 안됨
      * Hypermedia as the Engine of Application State (HATEOAS)
        * 모든 Hypermedia 표현은 그것을 통해 상태를 바꿀 수(관리할 수) 있어야 함 -> Link
        * Client가 어떤 정보를 요청할 때, Server는 그 값을 그대로 전달하지 않음\
          그 값을 받을 수 있는 URI를 전달함
        * Client는 전달받은 URI로 원하는 정보에 접근 가능함
        * 보통은 특정 메소드의 응답 결과를 다 URI로 반환하지는 않고, API Docs 등을 통해 정의함 -> \~한 상태가 되려면 \~ 메소드를 \~하게 써라 라는 가이드
        * 개념 이해에 도움이 된 블로그
          * [https://brunch.co.kr/@purpledev/29](https://brunch.co.kr/@purpledev/29)
  * Layered Architecture
    * Client - Server 사이에 Connector, Cache 등을 포함한 구성으로 구현 가능
  * Code-On-Demand
* Resource (추상적 개념)
  * 어떤 시기에든 통용 가능한, 해당 소프트웨어가 사용하는 Entity 집합
  * 내부에서 어떤 일이 벌어지든 Resource 라는 개념은 불변함
  * ex: Entity를 통해 만들어낸 Instance가 어떤 값을 가지건 다 같은 Resource로 가정하여 접근
    * /board/1 -> 안에 어떤 내용이 들어있다 해도 항상 boardId 1번인 Board 객체를 나타냄
* Representation
  * Data -> Metadata -> Meta-metadata 등을 통해 상태나 특성을 최대한 구체적으로 설명
  * 이를 위해 주로 사용하는 것이..  HTTP message
    * 리소스 조작 방법 : Method
    * 리소스의 형태, 내용 : Content type(Body의 형식을 정의), body
* 하지만.. Resource와 Representation과 실제 데이터는 모두 구분된 개념임
* REST는 Web을 활용하는 일반적인 경우에 통용 가능하나, API 만들 때 적용하면 특히 편리함
  * 현업에서 Level 4까지 적용하는 것은 어려우나, 통상적으로 Level 2 + Collection Pattern 구현 시 REST API하고 칭함 (Level 3 \~ 4는 현실에서 지키기 어려움)
