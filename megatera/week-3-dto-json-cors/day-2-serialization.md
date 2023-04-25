---
description: Day 2 Serialization
---

# Day 2 Serialization

### Serialization (직렬화)

* 객체 그 자체를 DB에 저장하거나 네트워크 저장하는 것은 불가능
* 객체의 구성을 도착지에서 복구할 수 있도록 데이터화하여야 함
* 직렬화 : 추후 역직렬화(Deserialization)을 통해 객체나 데이터(DTO 등)의 복사본을 만들 수 있도록 하는 기술
* 마샬링(Mashalling) : 직렬화 개념을 포함하여, 추후 언마셜링 시 원격 시스템의 객체를 복원할 수 있도록 하는 기술\
  (원격 시스템을 조정하여 동일한 형태의 객체를 다시 만들어 낼 수도 있으면 마셜링이라고 함)
* 따라서 직렬화는 마셜링의 부분집합\
  (직렬화-원격 복사본을 만들수 있도록 함\
  마셜링-원격을 리모트 컨트롤하여 동일한 객체를 새로 만드는 것도 포함)
* ex> Class -> 직렬화하지 않으면 클래스 이름과 메모리 주소밖에 알 수 없음\
  그러나 toString() 메소드를 사용하여 해당 클래스를 직렬화하면,\
  안에 어떤 내용이 있는지 확인할 수 있으므로 클래스 내용을 어디서든 복구 가능함
* 직렬화하는 형식
  * Binary String : Byte Stream을 활용
  * String (일반 문자열) : 기계가 parsing 가능
    * XML, JSON, yml, GraphQL 등.. (과거에는 SOAP라는 것도 썼다고 함 -> 속도가 느리다고 함)
      * node.js : package.json 등을 통해 프로젝트 환경설정을 하기도 함

### JSON

* JavaScript Object Notation
* [https://www.json.org/json-ko.html](https://www.json.org/json-ko.html)
* 경량의 Data 교환 형식
* JSON의 직렬화-역직렬화
  * 생성 : DTO(데이터) -> 변환기 -> JSON 문자열
  * 해석 : JSON 문자열 -> 변환기 -> DTO(데이터)
  * JavaScript 언어 자체는 object를 key-value 쌍으로 관리하므로해당 형식을 바로 처리할 수 있지만, 보안 문제 컨트롤을 위해 별도 라이브러리를 사용함
    * JSON.parse() -> 역직렬화
    * JSON.stringify() -> 직렬화
  * Java에서는 JavaScript에서의 object 관리 방식을 Map 등을 통해 구현할 수 있지만,\
    가급적 DTO로  JSON을 통해 F/E로부터 받은 스키마를 관리하고 타입 안정성을 확보함
* JSON 객체 -> Getter를 사용하여 구성 field의 값을 읽어냄
* JSON 스키마로서의 DTO
  * Tier간 remote 통신이 아닌 layer간, 내부 공개 인터페이스를 통한 데이터 전송 시에도 DTO 사용 가능
  * 하지만 기본적으로 JSON 스키마로서 DTO를 사용하는 경우가 많으며, 이는 B/E 입장에서 F/E와의 소통을 중시한 것
  * 필요한 영역(레이어)마다 별도 DTO 객체를 정의하여 사용하는 것은 자유.. (이나 권장 사항은 아닌 것 같음)
