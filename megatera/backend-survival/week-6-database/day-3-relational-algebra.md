---
description: Day 3 Relational Algebra
---

# Day 3 Relational Algebra

### Relation Operation

* Relation 연산
  * 하나 이상의 Relation으로 새로운 relation 만들기
    1. 단항 연산 : Selection, Projection, Rename
    2. 이항 연산 : Cartesian Product, Set Union, Set Difference
  * 연산을 통해 나온 결과물은 모두 다 새로운 Relation임

### Projection

* 주어진 relation에서 하는 attribute를 포함하는 pair로만 tuple을 구성
  * Relation의 탐색 범위를 Column 방향(가로)으로 좁혀줌

```sql
SELECT name, age, gender FROM people;
```

* 별도 attribute 제한을 하지 않는 경우, wildcard(\*) 사용

```sql
SELECT * FROM people;
```

### Selection

* 주어진 술어(조건)을 만족하는 튜플만 선택함
  * Relation의 탐색 범위를 Row 방향(세로)으로 좁혀줌
  * 술어 : \~는 \~하고, \~는 \~하고\~\~ 등으로 조건을 서술해준 표현 -> 이를 SQL로표현한 것이 WHERE
* 논리식에서는 σ (sigma) 로 나타냄

```sql
SELECT name FROM people WHERE age > 20;
```

### Cartesian Product (곱집합)

* 일반적으로 선형대수에서 이야기하는 곱집합과는 약간 다르게 구현되지만, 이렇게 표현됨
  * r1에 a1, a2 row 두 개가 있고, r2에 b1, b2 row가 두 개 있다고 할 때..
  * r1 x r2  = (a1 , b1), (a1, b2), (a2, b1), (a2, b2) 등 총 4개 row 가 생김
  * r1 x r2는 r1과 r2의 column을 그대로 이어붙인 형태로 연결됨

```sql
SELECT * FROM r1, r2;
```

* 보통 곱집합 단독으로 사용하지는 않으며, Selection과 함꼐 사용함
  * Cartesian Product + Selection = Join

$$
σ _{people.name} = {}_{items.personName}(People × items)
$$

```sql
SELECT p.name AS people_name
FROM people AS p, items as i
WHERE p.name = i.person_name;

SELECT p.name AS people_name
FROM people AS p
JOIN items AS i ON p.name = i.person_name;

// 둘 다 같은 결과를 도출한다.
```

* LEFT OUTER JOIN : FROM 다음에 나오는 table 기준으로 나머지 테이블에 해당 값이 없어도 join
* RIGHT OUTER JOIN : JOIN 다음에 나오는 table 기준으로 나머지 테이블에 해당 값이 없어도 join

#### Tips

* SQL query를 작성할 때 테이블-변수명이 대문자인 경우 그 외 query는 소문자로, 반대의 경우 query를 대문자로 작성

```sql
select ID, TITLE from ITEMS;
```

* 데이터베이스 정규화 관련 부분도 공부할수록 도움이 됨\
  (테이블간 중복 데이터 최소화를 통한 DB 최적화 등 위해..)
