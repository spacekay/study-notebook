---
description: 'Task: Connecting with Database'
---

# Task: Connecting with Database

{% embed url="https://github.com/megaptera-kr/backend-survival-week06/pull/12" %}

### 새롭게 배운 점

* Windows에서의 PostgreSQL configuration file 위치

```
C:\Program Files\PostgreSQL\{version}\data\postgresql.conf
```

* 람다 함수 내부에서 만들어진 객체들은, 외부의 객체들과 분리되어 있는 것 같다. 따라서 람다 함수 내부에서 만들어진 객체를 한번 return 해줘야 함

```java
@Override
public List<PostDto> findAll() {
    String query = "SELECT * FROM posts";

    return jdbcTemplate.query(query, resultSet -> {
        List<PostDto> newPosts = new ArrayList<>();
        while (resultSet.next()) {
            PostDto postDto = getDataFromRow(resultSet);
            newPosts.add(postDto);
        }
        return newPosts; // 내부에서 한 번 리스트 객체를 return한 뒤 전역에서 retun해주기
    });
}
```

### 어려웠던 점

* 예전 로컬에 PostgreSQL을 깔았어서 5432번 포트 사용이 불가함을 뒤늦게 알았다.
* JDBC를 넣기 시작하면 빌드 과정에서 다채로운 에러들이 생기기 시작한다.
* DBMS가 바뀔 때마다 SQL 문법은 다시 확인하자.

#### Tips

* Windows에서 강의에 나오는 것처럼 Postico를 설치하는 것은 어려움이 있으며, 그냥 pgAdmin을 까는 것이 좋음
