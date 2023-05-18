---
description: Day 2 URI & MIME type
---

# Day 2 URI & MIME type

* 평소 Web 관련 개념에 대해 구체적인 설명을 보고 싶을 때는, MDN Web Docs를 애용하자.
  * [https://developer.mozilla.org/ko/](https://developer.mozilla.org/ko/)

### URI

* Uniform Resource Identifier-식별자- : Resource를 식별하는 방법
  * URL : Location
  * URN : Name (unique) -> 도서, 음반, RFC 카드, 내부망 시스템등

### MIME

* Multipurpose Internet Mail Extensions -> 메일 시스템에서 발생한 개념
* 쉽게 말하면 Cotent type 또는 Media type (IANA에서 관리)
* 상대방에게 자신이 보내는 문서의 다양성을 '표현'하기 위한 양식
* type/subtype
* 주요 타입들
  * text/plain
  * text/html
  * text/css
  * text/javascript
  * application/xml
    * 범용, 상대적으로 덜 자기서술적임
      * 이것만 봐서는 정확히 어떤 xml 양식인지 파악이 어려움.\
        다만 xml은 파일 초반의 metadata 영역에서 표현을 좀 더 상세하게 해주는 경우가 많음
      * 상세 타입 예시) application/atom+xml
  * application/json
    * 범용, 매우 자기서술적이지 않음
      * 이것만 봐서는 그냥 json data라는 것 말고는 아무것도 알 수 없음
      * 상세 타입 예시) application/@@@+json
