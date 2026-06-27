```sql
-- 01. 나이가 21살 초과인 학생을 모두 조회하시오.

SELECT * FROM students
WHERE age > 21;
```

```sql
-- 02. 전공이 컴퓨터소프트웨어인 학생을 이름 오름차순으로 정렬하시오

SELECT * FROM students
WHERE major = '컴퓨터소프트웨어'
ORDER BY age;
```

```sql
-- 03. 이름에 '이'가 들어가는 학생을 조회하시오

SELECT * FROM students
WHERE name LIKE '%이%';
```

```sql
-- 04. 나이순 정렬 후 가장 어린 학생 2명만 조회하시오

SELECT * FROM students
ORDER BY age
LIMIT 2;
```