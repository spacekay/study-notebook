---
description: Day 6 JdbcTemplate
---

# Day 6 JdbcTemplate

### JdbcTemplate

* Spring Boot에서 DB 연결하기 위한 준비물
  1. JDBC API
  2. DBMS별 Driver

#### CommandLineRunner

* Spring Boot Command를 이용한 DB 초기 setup 코드 작성을 위한 interface
* 상속하여 클래스 작성 후 run() 메소드를 override하여 정의하면 그것대로 서버 부팅 시 실행
  * @SpringBootApplication 클래스에 @Bean으로 추가해도 되지만, 유지보수 편의를 위해서 보통 분리
* jdbcTemplate.query()

```
jdbcTemplate.query(query, resultSet -> {}, variables)
```

* jdbcTemplate.update()
  * return 값은 int로, 작업에 실패할 경우 0을 return

```
jdbcTemplate.update(query, variables)
```

* LIKE의 변수 ?의 값을 정의하는 경우, \*(wildcard)가 아니라 %를 이용하여야 함

```java
@Override
public void run(String... args) throws Exception {
    String query = "SELECT * FROM people LIKE ?";

    jdbcTemplate.query(query, resultSet -> {
        while (resultSet.next()) {
            System.out.println(resultSet.getString("name"));
        }
    }, "%종희%");
    
    query = "INSERT INTO people(name, age)" +
            " VALUES (?, ?)";
    jdbcTemplate.update(query, "홍길동", 20);
}
```

### TransactionTemplate

* Update를 포함하는 작업 과정 중 exception 발생 시, 변경사항을 저장하지 않고 Rollback 가능하게 함
* 작업이 완료된 경우 변경 내용을 Commit함

```java
transactionTemplate.execute(status -> {
    String query = "SELECT * FROM people";
    ...
    ...
});
```
