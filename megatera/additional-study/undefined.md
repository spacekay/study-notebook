---
description: https://school.programmers.co.kr/learn/courses/30/lessons/276035
---

# 비트 연산이 필요한 경우

이진수로 값을 변환하여 비교할 경우가 있는 경우, 비트연산을 활용하면 매우 간단해진다.

### 숫자를 이진수로 변환하여 문자열이라 생각하고 작성한 쿼리

```sql
SELECT DISTINCT id, email, first_name, last_name
FROM developers d
INNER JOIN skillcodes s 
ON substring(conv(d.skill_code, 10, 2), 
             -length(conv(s.code, 10, 2)),1) = '1'
WHERE category like 'Front%'
ORDER BY id;
```

### 이진수로 가정하여 비트 연산만을 활용해 작성한 쿼리

```sql
SELECT DISTINCT id, email, first_name, last_name
FROM developers d
INNER JOIN skillcodes s 
ON d.skill_code & s.code
WHERE category like 'Front%'
ORDER BY id;
```

앞으로 DB 설계 시에 비트 연산을 활용하여 좀더 편리하게 정규화가 가능할 수도 있다는 점을 기억해야 겠다.
