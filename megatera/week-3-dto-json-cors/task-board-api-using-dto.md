---
description: 'Task: Board API using DTO'
---

# Task: Board API using DTO

{% embed url="https://github.com/megaptera-kr/backend-survival-week03/pull/17" %}

#### 새로 배운 점

* Frondend - Backend 서버 동시에 돌리면서 RESTful하게 요청 응답 주고 받기 (기존에는 Backend에서 다하는 방식으로만 구현해 봄)
* CORS 설정 구현 (프로젝트 전역 config용 Class 활용)

```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("http://127.0.0.1:8000")
                .allowedOrigins("http://localhost:8000")
                .allowedMethods("GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS");
    }
}
```

#### 어려웠던 점

* stream()으로 구현하면 간단한 것도 직접 하나하나 코딩하면 엄청나게 길이가 길어졌다. 써본지 오래되서 안쓰고 해보려다 결국 다시 복습하는 계기가 되었다.

```java
PostDto postDto = postDtos.stream()
                                .filter( p -> p.getId().equals(id))
                                .findFirst().orElseThrow(Exception::new);
```

* Test tool을 셋업하는 과정에서 시행착오가 있었다. npm test를 무한히 던지면서 왜 안되지.. 고민하다가 에러 메시지들을 조금 더 읽어보니 '이 명령어를 써라'라고 적혀있는 것을 뒤늦게 깨닫게 되었다.

```bash
# npm 개발 환경 구축 (로컬)
npm install
# package-lock.json이 바뀌어 있어서, rollback 해준 후 진행
npm test # 그냥 작동했더니 뭔가 에러남. 일단 서버를 안 켠듯 함
npm run serve # 서버부터 켜기
npm run codeceptjs # React Demo를 돌려서 Frontend를 활용해 REST API 테스트 진행
```
