---
description: Day 2 Data Access
---

# Day 2 Data Access

### 3-tier architecture

1. Presentation (UI Layer)
2. Domain (Application Layer)
3. Data

* Spring에서의 구현
  * 기존에는 리스트에서 id를 객체 하나씩 찾으면서 데이터에 접근하므로, 데이터의 규모가 커질수록 매우 느려지게 됨 -> Map을 활용하여 데이터 접근-처리 속도를 늘릴 수 있도록 개선해 보기
    * 데이터를 다루는 Class를 따로 만들기 ->  DAO (Data Access Object)
  * PostDAO interface를 구현하는 PostListDAO, PostMapDAO class 만들기 (package: daos)
    * Interface : field와 추상 메소드만 남김
    * Interface를 상속한 Class의 메소드 작성 시 @Override annotation 넣어주기
    * 완성 시, Service에서 여러 DAO 중 기능 상 더욱 유용한 것을 선택하여 사용 가능

```java
public interface PostDAO {
    
    List<PostDto> findAll();
    
    PostDto find(String id);
    
    void save(PostDto postDto);
    
    void delete(String id);
}


public class PostMapDAO {

    private Map<String, PostDto> postDtos;
    
    public PostMapDAO () {
    ... // 생성자 구현 
    }
    
    @Override
    public List<PostDto> findAll() {
        return new ArrayList<>(postDtos.values());
    };
}


// Application layer
public class PostService {
    ... // 필드, 생성자
    
    public List<PostDto> getPosts() {
        // 데이터를 다루는 로직도 분리함
        return PostMapDAO.findAll();
    }
}
```

| Controller                         | Service                                | DAO                   |
| ---------------------------------- | -------------------------------------- | --------------------- |
| details(String id)                 | getPost(String id)                     | find(String id)       |
| list()                             | getPosts()                             | findAll()             |
| create(PostDto postDto)            | createPost(PostDto postDto)            | save(PostDto postDto) |
| update(String id, PostDto postDto) | updatePost(String id, PostDto postDto) | save(PostDto postDto) |
| delete(String id)                  | deletePost(String id)                  | delete(String id)     |

