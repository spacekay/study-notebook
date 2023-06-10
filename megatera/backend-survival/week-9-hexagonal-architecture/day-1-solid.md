---
description: Day 1 SOLID
---

# Day 1 SOLID

### SOLID

* Uncle Bob - Agile Software Development에서 최초 제창
* 마이클 페더스 - 각 원칙의 앞글자를 따서 SOLID로 명명
* **객체 지향 프로그래밍** 시 고려해야 할 5개 원칙

1. SRP (Single Responsibility Principle) : 단일 책임 원칙
2. OCP (Open-Closed Principle) : 개방-폐쇄 원칙
3. LSP (Liskov's Substitution Principle) : 리스코프 치환 원칙
4. ISP (Interface Segregation) : 인터페이스 분리 원칙
5. DIP (Dependency Inversion Principle) : 의존관계 역전 원칙

### 단일 책임 원칙

* 객체의 응집도를 다루는 설계 원칙
* 한 클래스는 단 한 가지의 변경 이유만을 가져야 한다.
* 기능 1개 고칠 때 -> 클래스 1개만 고치기
* 테스트하기 쉽고, 재사용하기 편하며, 더 작고 이해하기 쉬운 클래스를 사용하게 됨

### 개방-폐쇄 원칙

* 버트런드 마이어가 최초 제창
* 클래스의 추상화를 통한 다형성 활용 유도
* 소프트웨어 개체는 확장에 대해 열려있어야 하고, 수정에 대해서는 닫혀있어야 함
  * 이는 객체의 추상화를 통해 달성 가능하며, Java에서는 Interface를 활용함
* Open : 모듈의 기능을 변경할 수 있어야 함
* Closed : 해당 모듈의 기능 변경을 위해 다른 모듈도 변경을 하는 일은 없어야 함

### 리스코프 치환 원칙

* 바바라 리스코프가 최초 제창
* 클래스의 추상화-다형성 활용 방식에 대한 원칙
* Subtype은 그것의 Base type으로 바꾸어도 작동이 가능함 => 치환 가능
  * 이를 위해 instanceof나 downcast가 필요하면 리스코프 치환 원칙을 만족하지 못함
* 클래스의 **행위**가 중점
* 예시
  * 만족하지 않는 예시 : Rectangle - Square
  * 만족하는 예시 : Java Collections (List, Set 등.. )
    * List -> HashSet으로 바꾸어도 사용하는 행위가 동일하고 결과도 동일하다면 리스코프 치환 원칙을 만족함
* [https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-LSP-%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B9%98%ED%99%98-%EC%9B%90%EC%B9%99](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-LSP-%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B9%98%ED%99%98-%EC%9B%90%EC%B9%99)
* 프로그램이 거대해져서 객체를 생성하는 곳(Factory 등)과 사용하는 곳이 멀어지면 이 문제에 관련한 버그에 자주 직면함 -> 이런 경우 객체를 상속하여 사용할 때 주의하여야 함
* 리스코프 치환 원칙을 적용할 수 없는 경우
  * Java에서 다형성 활용 시 클래스에 다른 상위 클래스를 extends 하는 것보다, 인터페이스를 implements 하여 사용하는 것이 바람직함
  * 특정 클래스의 기능을 재사용해야 하는 경우, 가급적 상속이 아닌 합성(Composition)으로 구현

### 인터페이스 분리 원칙

* 기능별로 인터페이스를 분리하여 관리하여야 함
* 분리된 여러 인터페이스들을 한데 모아 대표 인터페이스를 통해 정의함
  * 하나의 인터페이스가 여러 인터페이스를 구현 가능\
    (예시 : `interface ProductRepository extends FindProductRepository, SearchProductRepository, CommandProductRepository`)
* 서비스 인터페이스 : UseCase라는 제목으로 나타냄\
  (예시 : `class ProductService implements GetProductUseCase, CreateProductUseCase, UpdateProductUseCase, DeleteProductUseCase`)
  * 역시 기능별로 별도로 만든 후 하나의 서비스 클래스로 구현

### 의존관계 역전 원칙

* 상위 수준의 모듈은 하위 수준의 모듈에 의존해서는 안되며, 둘 모두 추상화에 의존하여 사용(연결)해야 함
  * 상위 수준 모듈 : 비즈니스 로직
  * 하위 수준 모듈 : DB 등 기술적 부분
* 추상화는 구체적인 사항에 의존해서는 안되며, 구체적인 사항은 추상화에 의존하여야 함
* 영향을 주는 것 : 의존의 대상
* 영향을 받는 것 : 의존하는 주체
* 예시
  * `jdbcCartRepository`(Infrastructure Layer -> 기술적 -> 하위 수준)가 `CartRepository`(Repository -> Domain Layer -> 상위 수준)를 의존하면, 의존성 방향이 역전되어 의존관계 역전 윈칙을 만족함

#### Tips (모각코 등)

* Axios
  * JSON 기반, 에러 처리에 용이함. JavaScript 라이브러리 중 하나
* Fetch
  * JavaScript 기본 탑재 API이므로, 속도가 더욱 빠름
