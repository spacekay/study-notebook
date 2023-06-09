---
description: Day 2 Strategic Design
---

# Day 2 Strategic Design

일반적으로 전략적 설계 -> 전술적 설계 원칙을 따라 DDD를 구현하며, 전략적 설계 원칙을 실행하기 위한 기술적 해결 방안이 전술적 설계.

### Strategic Design

1. Ubiquitous Language (보편 언어)
2. Bounded Context (제한된 컨텍스트-맥락-)
3. Subdomain (하위 도메인) - 적절하게 도메인 나누기

#### 보편 언어

* 프로젝트에 참여하는 모두(개발자, 비개발자)가 하나의 언어(어휘 모음)를 사용하여 소통할 수 있게 함
  * 설계하기 어렵거나 코딩하기 어려운 언어를 다듬어야 함
  * 이러한 과정에서 구성원들의 역량이 더욱 발전할 수 있음

#### 제한된 컨텍스트 (맥락)

* Account 같은 단어는, 사용자 관점의 맥락과 은행 거래 관점의 맥락에서 서로 다른 의미(계정, 계좌)로 사용됨
* 하나의 팀, 하나의 프로젝트에서는 맥락을 가급적 좁혀서 사용하는 어휘가 하나의 대상만을 지칭하게 할 수 있음
  * 맥락을 좁히는 과정 = 하나의 시스템을 Bounded Context로 나누는 과정
  * 큰 개발 조직에서는 Bounded Context 기준으로 팀을 분류하는 경우가 많음

#### 하위 도메인 (설정)

| Subdomain                               | Bounded Context                            |
| --------------------------------------- | ------------------------------------------ |
| 도메인을 나누는 단위                             | 시스템을 나누는 단위                                |
| 현실을 조직화                                 | 소프트웨어를 조직화                                 |
| <p>개발하면서 맞춰나가야 함<br>(Problem Space)</p> | <p>개발하면서 발전시킬 수 있음<br>(Solution Space)</p> |

비즈니스 핵심 도메인을 발굴하여 발전시킬 수 있도록 전술적 설계에도 반영하여야 함
