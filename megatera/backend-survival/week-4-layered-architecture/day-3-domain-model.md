---
description: Day 3 Domain Model
---

# Day 3 Domain Model

### 4-tier architecture

* 사용자 인터페이스 (UI Layer)
  * Presentation
* 응용 (Application Layer)
  * Service
* 도메인 모델 (Domain Layer)
  * Domain Object
* 인프라스트럭처 (Data Layer)
  * Data Mapper
  * Data Access
* 사용자 인터페이스 - 인프라스트럭처 간에도 low-level에서는 연결되어 있으나,\
  가급적 중간 레이어를 나누어서 구현하고자 함.

### Application Layer - Domain Layer 분리

* Application layer : 소프트웨어가 수행할 작업을 정의함
  * 주요 특징
    * 얇게 유지됨 (코드량 적음)
    * 업무 규칙이나 지식을 담지 않음 (비즈니스 로직 없음)
    * 오직 작업을 조정할 뿐임
    * 도메인 객체의 협력자(Instance)에게 작업을 위임함\
      \-> 실질적 작업은 도메인 객체가 하도록 함
* Domain Model : 해결해야 하는 문제의 범위를 정의하는 일반적 객체로, '행위'가 발생하는 것이 중요
  * "**행위**"란?
    * 객체 내에서 직접 로직을 수행하며, 외부에서는 어떻게 로직이 동작하는지 알 수 없음
    * 그래도 Application은 Domain Model을 믿고 작업을 맡김 (협력)
    * Getter, Setter를 사용하여 Application에서 직접 연산하는 것 -> "행위"가 아님.
    * 행위 단위로 유닛 테스트 작성할 수 있음
  * Repository
    * 도메인 모델을 관리하는 객체 -> Domain Layer를 Data Layer와 연결할 수 있도록 함

### Data-Driven vs. Domain-Driven Design

<table><thead><tr><th width="222.33333333333331">항목</th><th width="250">Data-Driven (As-Is)</th><th>Domain-Driven (To-be)</th></tr></thead><tbody><tr><td>Application - Data 연결</td><td>DAO - VO(DTO)</td><td>Repository - Domain Model</td></tr><tr><td>중심 레이어</td><td>Database</td><td>Domain Model</td></tr><tr><td>특징</td><td><ul><li>Database를 고치면 프로그램을 고쳐야 함</li></ul></td><td><ul><li>프로그램을 고치고 이를 지원하기 위해 Database를 고침</li><li>CQRS, Event Sourcing 도입에 유리함</li><li>Data solution 변경이 비교적 쉬움<br>(ex: SQL -> NoSQL)</li></ul></td></tr></tbody></table>

* CQRS
  * Command and Query Responsibility Segregation
    * Command - Query 간 책임 분리
* Event Sourcing
  * [https://sabarada.tistory.com/231](https://sabarada.tistory.com/231)
  * Application의 모든 상태를 일으키는 이벤트를 순서에 맞게 정리하여, 상태에 맞는 status를 만들어 냄
  * 데이터(object)의 상태가 변경되는 모든 과정을 저장함 (NoSQL이든 RDB든)



### Anemic vs. Rich Domain Model

* CRUD만 하는 경우에는 Anemic Domain Model을 좀더빨리 만들 수 있음
* 하지만 프로그램을 유지보수할수록 Anemic Domain Model을 사용한 경우가 훨씬 빠르게 복잡도가 증가하며 작업 속도도 증가

#### 참고 자료

* 조영호, <오브젝트>
* 마켓컬리 기술 블로그 : [https://helloworld.kurly.com/blog/road-to-ddd/](https://helloworld.kurly.com/blog/road-to-ddd/)
  * 테스트 코드를 작성하면서 개발하는 것은 기본 전제.

### Domain Model 구현

* models, repositories 두 개의 package 생성
* **Model**
  * 다루고자 하는 Domain의 field와 생성자부터 정의
  * Field별 Model class를 별도로 생성해서 관리하는 것이 이상적이긴 함 -> Rich해질수록 좋음
    * 프로그램을 개선 시 Model과 그에 관련한 class의 코드 확인만으로 대부분 대응 가능
  * 특정 객체를 Map의 key object로 사용하기 위해서는, equals()와 hashCode() 메소드가 override 되어 있어야 함
  * Getter, Setter 사용하지 않음 -> 모델이 직접 비즈니스 로직을 구현하는 행위를 하도록 함
    * Getter는 가급적중간 상황 확인 목적 혹은 단순 값 열람 목적으로만 사용

```java
public class Post {
    
    private PostId id;
    
    private PostTitle title;
    
    public Post(PostId id, PostTitle title) {
        this.id = id;
        this.title = title;
    }
}
```

* **Repository**
  * Domain model -  Database 사이에서의 Data mapping & date access 담당
  * 기존 DAO의 기능을 Domain model과 연결시킴

```java
public class PostRepository {

    private Map<PostId, Post> posts;
    
    public Post find();
    
    public List<Post> findAll();
    
    public void save(Post post);
    
    public void delete(String id);
}
```

#### 구현 과정에서의 팁

* static method를 간단히 구현하여 생성자 대신 사용할 수도 있음&#x20;

```java
public static PostId of(String id) {
    return new PostId(id);
}
```

* 생성자만 구현되어 있다면, A 객체에서 B 객체로 전환해주는 코드를 stream().map(\~\~)으로 작성 가능
  * [https://isntyet.github.io/java/java-stream-%EC%A0%95%EB%A6%AC(map)/](https://isntyet.github.io/java/java-stream-%EC%A0%95%EB%A6%AC\(map\)/)&#x20;

```java
// 단, 이를 위해서는 Post 객체를 parameter로 받는 PostDto 생성자가 필요
PostDto postDto = posts.stream()
                                .map(post -> new PostDto(post))
                                .toList();
```
