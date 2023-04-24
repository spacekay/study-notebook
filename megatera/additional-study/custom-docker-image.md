---
description: additional study - docker (2023. 4. 11. ~ 2023. 4. 13.)
---

# Custom Docker Image

예전에 만들었던 간단한 sqlite3 기반 Django project를 image로 만들고, container로 구현하여 실행하는 과정 정리하고자 하였음 -> 비용 발생

AWS ECS 실행 과정은 infra 측면 이해가 없이는 그냥 가이드 보고 따라하는 수준이어서, 별도 문서에 수업 중 필기했던 내용만 정리함

* Amazon ECS에서는 Task definition을 통해 작업을 지시
* Service -> 여러 Task를 관리함
* ECS 생성 시에는 계정에 IAM 권한 부여가 필요했음 (새 계정으로 할 때 구글링 필수)
* EC2 아래에 로드 밸런서(Elastic Load Balancer)를 돌려서 성능 향상 및 자원 효율화
* 로드 밸런서 붙이는 방법에 따라 롤링 업데이트, 블루/그린 배포 방식 존재
* Github Action에서 빌드한 이미지에 AWS Access 정보를 config시 넣으면 Github Action을 통한 Task definition 작성도 가능해짐
