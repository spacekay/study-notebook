---
description: Day 1 Layered Architecture
---

# Day 1 Layered Architecture

### Layered Architecture Style

* Multi-tier / n-tier architecture style이 정확한 명칭
* <mark style="background-color:blue;">**Business logic**</mark>을 별도의 계층으로 완전히 분리하여 <mark style="background-color:blue;">Database system</mark> - <mark style="background-color:blue;">Client</mark> 사이에 배치함
* 3-tier architecture
  * Presentation
    * Spring Web MVC : **Controller**
  * Logic(Domain) -> 해당 앱을 통해 해결하고자 하는 문제 영역을 Domain이라고 함
    * Spring Web MVC : **Application** (Service)
  * Data
    * Spring Web MVC : **Repository**
* 4-tier architecture
  * Presentation
  * Application
  * Business
  * Data Access

### 기능과 웹의 분리

| 항목                | 기능                             | 웹                               |
| ----------------- | ------------------------------ | ------------------------------- |
| Layer (추상적 개념)    | Application Layer              | UI Layer                        |
| 자동차로 비유           | 바퀴                             | 핸들                              |
| Spring Web MVC 구현 | Application package (Services) | Controller package (Interfaces) |

* 웹은 기능을 구현하기 위한 토대가 됨. 하지만 웹에서 실제 기능을 구현하지는 않음.
* 웹에서 기능을 전부 구현하게 되면 Controller가 너무 복잡해지고, 유지보수가 어려움.

### Identifier

* UUID (Universally Unique Identifier)
  * 소프트웨어 구축에 쓰이는 식별자 표준
  * Java에서 기본 지원하는 범용 고유 식별자
  * 문제점
    * 생성 순서가 ID값 자체의 순서와 맞지 않음 -> Not sortable\
      (ex: e로 시작하는 ID가 a로 시작하는 ID보다 늦게 생성되었다고 보장할 수 없음)

```java
String id = UUID.randomUUID().toString().replace("=", "");
```

* ULID (Universally Unique Lexicographically Sortable Identifier)
  * Lexicographical -> 사전적 순서의 (a->z, 1->9)
  * ID 내부에 timestamp를 포함하며, 별도 라이브러리로 Spring에 build.gradle 등으로빌드함

```java
String id = UlidCreator.getUlid().toString();
```

* TSID (Time-Sorted Unique Identifier)
  * ULID spec을 따름
  * ULID보다 더 짧지만 time-sorted 되어 있음을 보장함
  * 사용 방식이 ULID와 거의 동일하며, ULID보다 지속적으로 라이브러리 관리 중인 것으로 추정

```java
String id = TsidCreator.getTsid().toString();
```

* 토막상식
  * Gradle project에 신규 패키지를 추가한 뒤에는, devtools가 서버를 재실행해주는 수준으로는 신규 패키지 추가 내용이 반영되지 않음.
  * 반드시 서버 전체를 다시 bootRun 해주어야 함

### Business Logic 분리

* Application package를 만들어서 관리
  * Class명은 Service로 유지
  * Controller class에 Service class를 field로 선언하고, Controller 생성자에 Service 객체를 주입

```java
@RequestMapping("/posts")
@RestController
public class PostController {

private PostService postService;

public PostController(PostService postService) {
    // Spring Container에서 관리하는 객체를 주입받기 위해 생성자 내부에서 new 하지 않음음
    this.postService = postService;
}

@GetMapping
public List<PostDto> list() {
    return postService.getPosts();
}

@GetMapping("/{id}")
public PostDto details(
        @PathVariable String id
    ) {
    return postService.getPost();
};

...

}


public class PostService {
    
    List<PostDto> postDtos = new ArrayList<>();
    
    // 실제 비즈니스 로직은 PostService Class를 통해서만 구현
    public List<PostDto> getPosts() {
        return postDtos;
    };
    
    public PostDto getPost(String id) {
        return findPost(id);
    }
    
    // GET, PUT, PATCH, DELETE 등 다양한 메소드 실행 시 반복 사용되는 기능을 분리함
    private findPost(String id) {
        return postDtos.stream()
                        .filter(p-> p.getId().equals(id))
                        .findFirst()
                        .orElseThrow(PostNotFound::new);
    }
}
```

* Refactoring
  * 관심사의 분리에 따라 코드도 분리하여야 하는데, 코드를 어떻게 배치하느냐는 설계의 문제.
  * 기능이 작동하는 방식은 바뀌지 않고, 코드 설계만 개선하는 경우를 Refactoring이라 함.
  * Method / Class refactoring
  * 해당 Class 내에서 반복 사용되는 코드는 private method로 분리하여 관리 -> Refactoring
    * 해당 기능은 새로 만든 그 메소드로만 관리하면 클래스 내 해당 기능 실행 포인트 전체를 관리할 수 있음 -> 기능 단위 **관리 포인트 최소화**.
