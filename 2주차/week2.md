# SQLD 스터디 2주차 — JOIN, GROUP BY, 집계 함수, HAVING, 서브쿼리

## 이번 주 목표

- 지난 주 실습 문제 정답 확인
- JOIN으로 여러 테이블 합쳐서 조회하기
- GROUP BY와 집계 함수로 데이터 그룹화하기
- HAVING으로 그룹 조건 걸기
- 서브쿼리로 쿼리 안에 쿼리 작성하기

---

## 지난 주 실습 문제 정답

### Q1. 나이가 21살 초과인 학생을 모두 조회하시오.

```sql
SELECT * FROM students
WHERE age > 21;
```

### Q2. 전공이 컴퓨터소프트웨어인 학생을 이름 오름차순으로 정렬해서 조회하시오.

```sql
SELECT * FROM students
WHERE major = '컴퓨터소프트웨어'
ORDER BY name ASC;
```

### Q3. 이름에 '이'가 들어가는 학생을 조회하시오.

```sql
SELECT * FROM students
WHERE name LIKE '%이%';
```

### Q4. 나이순으로 정렬했을 때 가장 어린 학생 2명만 조회하시오.

```sql
SELECT * FROM students
ORDER BY age ASC
LIMIT 2;
```

---

## 실습 준비 — 테이블 추가 생성

이번 주는 테이블 두 개를 조인해서 사용할 예정입니다. 지난 주에 만든 `students` 테이블에 `scores` 테이블을 추가로 생성해 주세요.

```sql
CREATE TABLE scores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT,
    subject VARCHAR(50),
    score INT,
    FOREIGN KEY (student_id) REFERENCES students(id)
);

INSERT INTO scores (student_id, subject, score) VALUES
(1, 'SQL', 88),
(1, '데이터모델링', 92),
(2, 'SQL', 75),
(2, '데이터모델링', 80),
(3, 'SQL', 95),
(3, '데이터모델링', 70),
(4, 'SQL', 60),
(4, '데이터모델링', 65),
(5, 'SQL', 85),
(5, '데이터모델링', 90),
(6, 'SQL', 78),
(6, '데이터모델링', 82);
```

> `FOREIGN KEY`는 `scores.student_id`가 `students.id`를 참조한다는 선언입니다. 존재하지 않는 student_id를 넣으면 오류가 발생하니 주의하세요.

---

## 1. JOIN

JOIN은 두 개 이상의 테이블을 연결해서 한 번에 조회하는 문법입니다.

### JOIN의 종류

| 종류 | 설명 |
|------|------|
| `INNER JOIN` | 두 테이블에서 조건이 일치하는 행만 조회합니다 |
| `LEFT JOIN` | 왼쪽 테이블은 전부, 오른쪽은 일치하는 것만 조회합니다 |
| `RIGHT JOIN` | 오른쪽 테이블은 전부, 왼쪽은 일치하는 것만 조회합니다 |

### INNER JOIN

```sql
SELECT students.name, scores.subject, scores.score
FROM students
INNER JOIN scores ON students.id = scores.student_id;
```

`ON` 뒤에 어떤 컬럼을 기준으로 연결할지 조건을 작성합니다. `students.id = scores.student_id` 가 일치하는 행끼리 합쳐집니다.

### 별칭(Alias) 사용하기

테이블 이름이 길 때 별칭을 붙여서 짧게 쓸 수 있습니다. `FROM students s` 에서 `s` 가 별명이에요.

```sql
-- AS 키워드를 명시하는 방법
FROM students AS s

-- AS를 생략하는 방법 (둘 다 동일하게 동작합니다)
FROM students s
```

별칭을 한 번 정하면 그 쿼리 안에서는 반드시 별칭으로만 써야 합니다. 원래 이름을 섞어 쓰면 오류가 발생합니다.

```sql
-- 별칭 없을 때
SELECT students.name, scores.subject, scores.score
FROM students
INNER JOIN scores ON students.id = scores.student_id;

-- 별칭 있을 때 (결과는 동일, 훨씬 간결합니다)
SELECT s.name, sc.subject, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id;
```

컬럼에도 똑같이 별칭을 붙일 수 있습니다. 결과 컬럼 이름을 바꾸고 싶을 때 씁니다.

```sql
SELECT AVG(score) AS 평균점수
SELECT COUNT(*) AS 학생수
```

**WHERE와 같이 쓰기**

```sql
-- SQL 과목만 조회
SELECT s.name, sc.subject, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
WHERE sc.subject = 'SQL';
```

### LEFT JOIN

```sql
SELECT s.name, sc.subject, sc.score
FROM students s
LEFT JOIN scores sc ON s.id = sc.student_id;
```

점수가 없는 학생도 왼쪽 테이블(students) 기준으로 전부 나오고, 점수가 없으면 `NULL`로 표시됩니다.

> INNER JOIN과 LEFT JOIN의 차이: 점수 데이터가 없는 학생이 있을 때 INNER JOIN은 해당 학생이 아예 조회되지 않고, LEFT JOIN은 NULL로 표시되면서 조회됩니다.

---

## 2. GROUP BY

GROUP BY는 특정 컬럼을 기준으로 데이터를 묶어줍니다. 집계 함수와 함께 사용하는 것이 기본입니다.

### 집계 함수 정리

| 함수 | 설명 |
|------|------|
| `COUNT(*)` | 행 개수를 셉니다 |
| `SUM(컬럼)` | 합계를 구합니다 |
| `AVG(컬럼)` | 평균을 구합니다 |
| `MAX(컬럼)` | 최댓값을 구합니다 |
| `MIN(컬럼)` | 최솟값을 구합니다 |

### 전공별 학생 수

```sql
SELECT major, COUNT(*) AS 학생수
FROM students
GROUP BY major;
```

`AS`는 컬럼에 별칭을 붙이는 키워드입니다. 결과 컬럼 이름이 `학생수`로 표시됩니다.

### 학생별 평균 점수

```sql
SELECT s.name, AVG(sc.score) AS 평균점수
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
GROUP BY s.id, s.name;
```

### 과목별 최고 점수 / 최저 점수

```sql
SELECT subject, MAX(score) AS 최고점수, MIN(score) AS 최저점수
FROM scores
GROUP BY subject;
```

### ROUND — 소수점 반올림

평균을 구하면 소수점이 길게 나오는 경우가 많습니다. `ROUND` 함수로 원하는 자릿수까지 반올림할 수 있습니다.

```sql
ROUND(값, 자릿수)
```

```sql
-- 소수점 첫째 자리까지 반올림
SELECT subject, ROUND(AVG(score), 1) AS 평균점수
FROM scores
GROUP BY subject;

-- 자릿수를 0으로 하면 정수로 반올림
SELECT ROUND(AVG(score), 0) AS 평균점수 FROM scores;

-- 자릿수를 음수로 하면 정수 자리에서 반올림
SELECT ROUND(1234, -2);  -- 결과: 1200
```

> 실습 문제 Q3처럼 "소수점 첫째 자리까지"라는 조건이 나오면 `ROUND(..., 1)` 을 쓰면 됩니다.

---

## 3. HAVING

WHERE는 GROUP BY 이전에 조건을 걸고, HAVING은 GROUP BY 이후에 조건을 겁니다.

```
SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY
```

### 평균 점수가 80점 이상인 학생만 조회

```sql
SELECT s.name, AVG(sc.score) AS 평균점수
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
GROUP BY s.id, s.name
HAVING AVG(sc.score) >= 80;
```

> `WHERE AVG(sc.score) >= 80` 으로 작성하면 오류가 발생합니다. 집계 함수 결과에 조건을 걸 때는 반드시 `HAVING`을 사용해야 합니다.

### 학생 수가 1명 초과인 전공만 조회

```sql
SELECT major, COUNT(*) AS 학생수
FROM students
GROUP BY major
HAVING COUNT(*) > 1;
```

---

## 4. 서브쿼리

서브쿼리는 쿼리 안에 또 다른 쿼리를 넣는 것입니다. 괄호로 감싸서 사용합니다.

### WHERE 절 서브쿼리

전체 평균 점수보다 높은 점수를 받은 학생 조회

```sql
SELECT s.name, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
WHERE sc.score > (SELECT AVG(score) FROM scores);
```

괄호 안의 `SELECT AVG(score) FROM scores` 가 먼저 실행되어 전체 평균을 구하고, 그 값보다 높은 점수만 필터링합니다.

### SQL 과목에서 가장 높은 점수를 받은 학생 조회

```sql
SELECT s.name, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
WHERE sc.subject = 'SQL'
AND sc.score = (SELECT MAX(score) FROM scores WHERE subject = 'SQL');
```

### FROM 절 서브쿼리 (인라인 뷰)

학생별 평균 점수를 구한 뒤, 그 중 80점 이상인 학생만 조회

```sql
SELECT name, 평균점수
FROM (
    SELECT s.name, AVG(sc.score) AS 평균점수
    FROM students s
    INNER JOIN scores sc ON s.id = sc.student_id
    GROUP BY s.id, s.name
) AS avg_table
WHERE 평균점수 >= 80;
```

FROM 절에 사용한 서브쿼리는 반드시 별칭(`AS avg_table`)을 붙여야 합니다.

---

## SQL 실행 순서 정리

SQL은 작성 순서와 실제 실행 순서가 다릅니다. 헷갈리지 않도록 정리해두세요.

```
1. FROM       — 테이블 가져오기
2. JOIN       — 테이블 연결
3. WHERE      — 행 필터링
4. GROUP BY   — 그룹화
5. HAVING     — 그룹 필터링
6. SELECT     — 컬럼 선택
7. ORDER BY   — 정렬
8. LIMIT      — 개수 제한
```

WHERE가 GROUP BY보다 먼저 실행되기 때문에, WHERE 단계에서는 아직 집계가 이루어지지 않은 상태입니다. 그래서 `WHERE AVG(...)` 처럼 집계 함수를 WHERE에 쓰면 오류가 발생합니다.

---

## 이번 주 실습 문제

1. `students`와 `scores` 테이블을 조인해서 학생 이름과 과목, 점수를 모두 조회하시오.
2. 학생별 총점을 구하고 총점 내림차순으로 정렬하시오.
3. 과목별 평균 점수를 구하시오. (소수점 첫째 자리까지)
4. 평균 점수가 80점 미만인 학생의 이름과 평균 점수를 조회하시오.
5. 전체 평균 점수보다 높은 점수를 받은 학생의 이름과 점수를 조회하시오. (서브쿼리 사용)
6. 학생 수가 2명 이상인 전공과 학생 수를 조회하시오.

---

## 다음 주 예고

- DDL (CREATE, ALTER, DROP)
- DML (INSERT, UPDATE, DELETE)
- 트랜잭션 (COMMIT, ROLLBACK)

---

> 이 글은 SQLD 스터디그룹 2주차 자료입니다. 실습하다 막히는 부분은 QnA 게시판에 남겨주세요.