# 2주차 개념 쉽게 이해하기

> 강의자료가 어렵게 느껴지는 분들을 위한 보충 설명입니다. 비유를 통해 핵심 개념을 쉽게 정리해봤습니다.

---

## 1. JOIN — 두 엑셀 시트를 붙이는 것

학생 명단 시트(students)랑 점수 시트(scores)가 따로 있을 때, 학번을 기준으로 두 시트를 옆에 붙여서 "이름 + 점수"를 한 번에 보는 게 JOIN입니다.

```
students                    scores
id | name                   student_id | score
1  | 최진성        →        1          | 88
2  | 이준호        →        2          | 75
```

`ON s.id = sc.student_id` 는 "어떤 컬럼을 기준으로 붙일지" 지정하는 겁니다. 열쇠 구멍이라고 생각하면 됩니다.

### INNER JOIN vs LEFT JOIN

```
students 테이블         scores 테이블
최진성 (id: 1)    →    점수 있음  ✓
이준호 (id: 2)    →    점수 있음  ✓
홍길동 (id: 7)    →    점수 없음  ✗
```

- **INNER JOIN** — 양쪽 다 있는 것만. 홍길동은 아예 안 나옵니다.
- **LEFT JOIN** — 왼쪽(students) 기준으로 전부 나옵니다. 홍길동은 점수 자리에 NULL로 표시됩니다.

```sql
-- INNER JOIN: 점수 있는 학생만
SELECT s.name, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id;

-- LEFT JOIN: 점수 없는 학생도 포함
SELECT s.name, sc.score
FROM students s
LEFT JOIN scores sc ON s.id = sc.student_id;
```

---

## 2. GROUP BY — 서랍 정리

전체 데이터를 특정 기준으로 서랍에 나눠 담고, 각 서랍 안에서 집계를 내는 겁니다.

```
SQL 서랍        → 88, 75, 95, 60, 85, 78  →  AVG = 80.2
데이터모델링 서랍 → 92, 80, 70, 65, 90, 82  →  AVG = 79.8
```

집계 함수는 서랍 안에서 쓰는 계산기입니다.

| 함수 | 역할 |
|------|------|
| `COUNT(*)` | 서랍 안에 몇 개인지 셉니다 |
| `SUM` | 서랍 안의 값을 다 더합니다 |
| `AVG` | 서랍 안의 평균을 구합니다 |
| `MAX / MIN` | 서랍 안의 최고값 / 최저값을 구합니다 |

```sql
-- 과목별(서랍별) 평균 점수
SELECT subject, AVG(score) AS 평균점수
FROM scores
GROUP BY subject;
```

### ROUND — 소수점 반올림

AVG를 쓰면 소수점이 길게 나오는 경우가 많습니다. `ROUND(값, 자릿수)` 로 깔끔하게 반올림할 수 있습니다.

```sql
ROUND(80.166666, 1)  -- 결과: 80.2  (소수점 첫째 자리까지)
ROUND(80.166666, 0)  -- 결과: 80    (정수로)
ROUND(1234, -2)      -- 결과: 1200  (백의 자리에서 반올림)
```

```sql
-- 실습 문제 Q3 풀이
SELECT subject, ROUND(AVG(score), 1) AS 평균점수
FROM scores
GROUP BY subject;
```

---

## 3. HAVING vs WHERE — 필터가 두 개인 정수기

```
원수 → [WHERE 필터] → GROUP BY → [HAVING 필터] → 결과
```

- **WHERE** — 그룹 묶기 **전**에 거르는 필터. 개별 행에 조건을 걸 때 씁니다.
- **HAVING** — 그룹 묶은 **후**에 거르는 필터. 집계 결과에 조건을 걸 때 씁니다.

```sql
WHERE age > 20           -- 행 하나하나에 조건 → 정상
WHERE AVG(score) > 80    -- 집계 결과에 조건 → 오류 발생!
HAVING AVG(score) > 80   -- 집계 결과에 조건 → 정상
```

> 시험에서 이 둘을 바꿔 쓰게 해서 오답을 유도하는 문제가 자주 나옵니다. WHERE에는 집계 함수를 쓸 수 없다는 것을 꼭 기억하세요.

```sql
-- 평균 점수가 80점 이상인 학생만
SELECT s.name, AVG(sc.score) AS 평균점수
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id
GROUP BY s.id, s.name
HAVING AVG(sc.score) >= 80;
```

---

## 4. 서브쿼리 — 괄호 안의 계산기를 먼저 돌리는 것

수학에서 `5 + (3 × 2)` 를 계산할 때 괄호 안을 먼저 계산하는 것처럼, SQL도 괄호 안의 쿼리를 먼저 실행해서 그 결과를 바깥 쿼리에서 사용합니다.

```sql
WHERE sc.score > (SELECT AVG(score) FROM scores)
--                ↑ 이게 먼저 실행돼서 숫자 하나가 됨 (예: 80.2)
-- 결과적으로 이렇게 동작합니다:
-- WHERE sc.score > 80.2
```

### FROM 절 서브쿼리 (인라인 뷰)

FROM 절에 서브쿼리를 넣으면 임시 테이블처럼 사용할 수 있습니다.

```sql
SELECT name, 평균점수
FROM (
    SELECT s.name, AVG(sc.score) AS 평균점수
    FROM students s
    INNER JOIN scores sc ON s.id = sc.student_id
    GROUP BY s.id, s.name
) AS avg_table          -- 반드시 별칭을 붙여야 합니다
WHERE 평균점수 >= 80;
```

FROM 절 서브쿼리에 반드시 `AS avg_table` 같은 별칭을 붙여야 하는 이유는, SQL이 이 임시 테이블을 부를 이름이 필요하기 때문입니다. 별칭이 없으면 오류가 발생합니다.

---

## 5. 별칭(Alias) — 사람 별명

이름이 "김철수"인 사람을 매번 "김철수야"라고 부르지 않고 "철수야"라고 부르는 것처럼, 테이블이나 컬럼 이름이 길 때 짧게 별명을 붙여서 쓰는 겁니다.

### 테이블 별칭

```sql
-- AS 키워드를 명시하는 방법
FROM students AS s

-- AS를 생략하는 방법 (둘 다 동일합니다)
FROM students s
```

별칭을 한 번 정하면 그 쿼리 안에서는 **반드시 별칭으로만** 써야 합니다.

```sql
FROM students s
WHERE students.age > 20  -- 오류! 별칭을 쓰기로 했으면 별칭으로
WHERE s.age > 20         -- 정상
```

별칭을 쓰면 쿼리가 훨씬 짧고 읽기 쉬워집니다.

```sql
-- 별칭 없을 때
SELECT students.name, scores.subject, scores.score
FROM students
INNER JOIN scores ON students.id = scores.student_id;

-- 별칭 있을 때 (결과는 동일)
SELECT s.name, sc.subject, sc.score
FROM students s
INNER JOIN scores sc ON s.id = sc.student_id;
```

### 컬럼 별칭

테이블뿐만 아니라 컬럼에도 별칭을 붙일 수 있습니다. 결과 컬럼 이름을 바꾸고 싶을 때 씁니다.

```sql
SELECT AVG(score) AS 평균점수   -- 결과 컬럼 이름: 평균점수
SELECT COUNT(*) AS 학생수       -- 결과 컬럼 이름: 학생수
```

---

## 6. SQL 실행 순서 — 작성 순서와 다릅니다

SQL은 작성하는 순서와 실제 실행 순서가 다릅니다. 이걸 모르면 왜 오류가 나는지 이해하기 어렵습니다.

```
작성 순서       실제 실행 순서
SELECT      →   1. FROM     (재료 가져오기)
FROM        →   2. JOIN     (재료 합치기)
JOIN        →   3. WHERE    (불량품 걸러내기)
WHERE       →   4. GROUP BY (종류별로 분류)
GROUP BY    →   5. HAVING   (분류된 것 중 조건 걸기)
HAVING      →   6. SELECT   (필요한 것만 골라내기)
ORDER BY    →   7. ORDER BY (순서 정렬)
LIMIT       →   8. LIMIT    (개수 자르기)
```

WHERE가 GROUP BY보다 먼저 실행되기 때문에, WHERE 단계에서는 아직 집계가 이루어지지 않은 상태입니다. 그래서 `WHERE AVG(...)` 가 오류를 일으키는 거예요. 집계 함수는 GROUP BY 이후인 HAVING에서만 쓸 수 있습니다.

---

> 이 글은 2주차 강의자료의 보충 설명입니다. 추가로 궁금한 부분은 QnA 게시판에 남겨주세요.