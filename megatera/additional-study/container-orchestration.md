---
description: additional study - docker (2023. 4. 11. ~ 2023. 4. 13.)
---

# Container Orchestration

* Container Orchestration Tool을 사용하는 이유
  * Container clustering
    * 여러개의 노드를 하나의 클러스터로 묶어, application을 하나의 container가 아닌 여러 container로 분산 실행 및 운영
    * 여러 컨테이너를 하나의 서비스처럼 사용할 수 있음
  * Service Discovery
  * Auto Scaling
    * 트래픽에 따라 컨테이너 수 조절
  * Load Balancing
    * 각 컨테이너에 트래픽을 균등하게 분배
    * 특정 컨테이너에 트래픽 부하가 쏠리게 하지 않음
  * Roll Out & Roll Back
  * Auto Recovery
  * 보안 및 네트워크 관리
* Container Orchestration Tool 종류
  * Docker
  * AWS ECS, EKS
  * Kubernetes
  * GKS
  * etc..
* 기본적인 Container Orchestration Tool 소개
  * Docker Swarm
    * 간단하게 작동하고 설정이 쉬움
  * Kubernetes
    * Google이 개발하여 the Linux Foundation에서 관리하는 open source platform\
      (2015년 최초 발표)
    * 다양한 서비스와 대용량 트래픽을 커버하기 위해 개발
    * 스케일링 기능 - 서비스 디스커버리 기능
    * 다양한 환경에서 작동 가능 (VM 등)
    * Kubernetes Component
    *

        <figure><img src="../.gitbook/assets/components-of-kubernetes.svg" alt=""><figcaption><p>Kubernetes cluster components (추후 실사용 기회 있을 때 다시 공부..)</p></figcaption></figure>
    * [https://kubernetes.io/ko/docs/concepts/overview/components/](https://kubernetes.io/ko/docs/concepts/overview/components/)
  * GKS, AKS
    * 구글, 아마존에서 각각 서비스하는 Kubernetes service
  * ECS (Amazon Elastic Container Service)
    * Docker container를 배포, 관리, 스케일링 할 수 있는 서비스
    * 종류
      * EC2
        * AWS EC2를 생성한 뒤 그것을 통해 컨테이너 운영
        * 스케일링 할 때 EC2를 늘려서 활용
        * 트래픽이 적을 때도 꾸준한 고정 비용 발생
      * Fargate
        * Serverless이고, 사용자가 관리할 필요 없음
        * 시간당 vCPU, 스토리지 사용량 등 PC의 리소스를 사용한 만큼 비용 부과
        * 트래픽이 일정 이상이면 EC2보다 더 비싸짐
      * External
        * 거의 사용하지 않는 편
        * 호스트에서 AWS 외부 device에  ECS를 정의하여, AWS Console로 동작
    *   구성

        <figure><img src="../.gitbook/assets/overview-fargate.png" alt=""><figcaption><p>Fargate 기반 AWS ECS diagram</p></figcaption></figure>

        * [https://docs.aws.amazon.com/ko\_kr/AmazonECS/latest/developerguide/launch\_types.html](https://docs.aws.amazon.com/ko\_kr/AmazonECS/latest/developerguide/launch\_types.html)
        * 목요일 수업에서 직접 Fargate로 올려볼 Application image\
          \-> 작성 완료. 외부 DB 사용하는 케이스도 docker-compose.yml을 활용하여 구성 가능

