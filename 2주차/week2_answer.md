```sql
-- 01. students 와scores 테이블을조인해서학생이름, 과목, 점수를 모두 조회하시오.

SELECT s.name, sc.subject, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id;
```

```sql
-- 02. 학생별 총점을 구하고 총점 내림차순으로 정렬하시오.

SELECT s.name, SUM(sc.score) AS 총점
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
GROUP BY s.id, s.name
ORDER BY 총점 DESC;
```

```sql
-- 03. 과목별 평균 점수를 구하시오. (소수점 첫째 자리까지)

SELECT subject, ROUND(AVG(score), 1) AS 평균점수
FROM scores
GROUP BY subject;
```

```sql
-- 04. 평균 점수가 80점 미만인 학생의 이름과 평균 점수를 조회하시오.

SELECT s.name, AVG(sc.score) AS 평균점수
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
GROUP BY s.id, s.name
HAVING AVG(sc.score) < 80;
```

```sql
-- 05. 전체 평균 점수보다 높은 점수를 받은 학생의 이름과 점수를 조회하시오. (서브쿼리 사용)

SELECT s.name, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
WHERE sc.score > (SELECT AVG(score) FROM scores);
```

```sql
-- 06. 학생 수가 2명 이상인 전공과 학생 수를 조회하시오.

SELECT major, COUNT(*) AS 학생수
FROM students
GROUP BY major
HAVING COUNT(*) >= 2;
```