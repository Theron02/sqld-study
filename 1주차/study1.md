# 1주차 개념 쉽게 이해하기

> 강의자료가 어렵게 느껴지는 분들을 위한 보충 설명입니다. 비유를 통해 핵심 개념을 쉽게 정리해봤습니다.

---

## 1. 데이터베이스 / 테이블 — 엑셀이랑 똑같습니다

처음 들으면 용어가 낯설지만, 엑셀이랑 1:1로 비교하면 바로 이해됩니다.

| SQL 용어 | 엑셀 비유 | 설명 |
|----------|-----------|------|
| Database | 엑셀 파일 (.xlsx) | 데이터를 담는 가장 큰 단위 |
| Table | 엑셀 시트 (탭) | 데이터베이스 안의 표 하나 |
| Row (행) | 엑셀 가로 한 줄 | 데이터 하나 (학생 한 명) |
| Column (열) | 엑셀 세로 한 줄 | 속성 하나 (이름, 나이 등) |

```
students 테이블
┌────┬────────┬─────┬──────────────────┐
│ id │  name  │ age │      major       │  ← 컬럼 (열)
├────┼────────┼─────┼──────────────────┤
│  1 │ 최진성 │  25 │ 컴퓨터소프트웨어  │  ← 행 하나 = 학생 한 명
│  2 │ 이준호 │  20 │ 산업경영          │
│  3 │ 김광은 │  21 │ 게임VR            │
└────┴────────┴─────┴──────────────────┘
```

---

## 2. CREATE TABLE — 테이블 설계도 그리기

테이블을 만들 때 각 컬럼의 이름과 타입을 정의하는 게 스키마입니다. 건물을 짓기 전에 설계도를 그리는 것과 같아요.

```sql
CREATE TABLE students (
    id    INT PRIMARY KEY AUTO_INCREMENT,
    name  VARCHAR(50),
    age   INT,
    major VARCHAR(50)
);
```

### 자주 쓰는 타입

| 타입 | 설명 | 예시 |
|------|------|------|
| `INT` | 정수 | 나이, 점수, id |
| `VARCHAR(n)` | 최대 n글자 문자열 | 이름, 전공 |
| `TEXT` | 긴 문자열 | 게시글 본문 |
| `DATE` | 날짜 | 생년월일 |

### PRIMARY KEY와 AUTO_INCREMENT

- **PRIMARY KEY** — 각 행을 구분하는 고유 값입니다. 학교의 학번처럼 중복이 없어야 합니다.
- **AUTO_INCREMENT** — 행이 추가될 때마다 1씩 자동으로 늘어납니다. id를 직접 입력하지 않아도 돼요.

```sql
INSERT INTO students (name, age, major) VALUES ('최진성', 25, '컴퓨터소프트웨어');
-- id는 자동으로 1이 됩니다

INSERT INTO students (name, age, major) VALUES ('이준호', 20, '산업경영');
-- id는 자동으로 2가 됩니다
```

---

## 3. SELECT — 원하는 데이터 꺼내기

SELECT는 테이블에서 데이터를 조회하는 문법입니다. 엑셀에서 필터 걸어서 원하는 행만 보는 것과 같아요.

### 기본 구조

```sql
SELECT 컬럼명
FROM 테이블명
WHERE 조건
ORDER BY 정렬기준
LIMIT 개수;
```

### 전체 조회

```sql
SELECT * FROM students;
-- * 는 모든 컬럼을 의미합니다
```

### 특정 컬럼만

```sql
SELECT name, age FROM students;
-- 보고 싶은 컬럼만 골라서 조회합니다
```

---

## 4. WHERE — 조건 필터

엑셀 필터 기능이랑 똑같습니다. 조건에 맞는 행만 꺼내올 때 씁니다.

```sql
-- 나이가 22살 이상인 학생
SELECT * FROM students
WHERE age >= 22;

-- 전공이 컴퓨터소프트웨어인 학생
SELECT * FROM students
WHERE major = '컴퓨터소프트웨어';
```

### 조건 두 개 이상 — AND / OR

```sql
-- 나이가 20살 이상이고 전공이 컴퓨터소프트웨어인 학생
SELECT * FROM students
WHERE age >= 20 AND major = '컴퓨터소프트웨어';

-- 전공이 경영학이거나 전자공학인 학생
SELECT * FROM students
WHERE major = '경영학' OR major = '전자공학';
```

- **AND** — 두 조건 모두 만족해야 나옵니다
- **OR** — 둘 중 하나만 만족해도 나옵니다

---

## 5. 비교 연산자 — 조건 표현하는 방법

### 기본 연산자

| 연산자 | 의미 | 예시 |
|--------|------|------|
| `=` | 같다 | `WHERE age = 22` |
| `!=` 또는 `<>` | 다르다 | `WHERE age != 22` |
| `>` / `<` | 크다 / 작다 | `WHERE age > 20` |
| `>=` / `<=` | 이상 / 이하 | `WHERE age >= 20` |

> 주의: SQL에서 같다는 `=` 하나만 씁니다. `==` 를 쓰면 오류가 납니다.

### BETWEEN — 범위 조건

```sql
-- 나이가 20~22살 사이 (20, 21, 22 모두 포함)
SELECT * FROM students
WHERE age BETWEEN 20 AND 22;
```

`BETWEEN A AND B` 는 A 이상 B 이하를 의미합니다. A와 B 자체도 포함됩니다.

### IN — 여러 값 중 하나

```sql
-- 전공이 산업경영 또는 컴퓨터전자공학인 학생
SELECT * FROM students
WHERE major IN ('산업경영', '컴퓨터전자공학');
```

`OR` 를 여러 번 쓰는 것과 같지만 훨씬 짧게 쓸 수 있습니다.

```sql
-- 아래 두 쿼리는 결과가 완전히 동일합니다
WHERE major IN ('산업경영', '컴퓨터전자공학')
WHERE major = '산업경영' OR major = '컴퓨터전자공학'
```

### LIKE — 특정 문자 포함

```sql
-- 이름에 '진'이 들어가는 학생
SELECT * FROM students
WHERE name LIKE '%진%';
```

`%` 는 와일드카드로 "아무 문자나 0개 이상"을 의미합니다.

```
'%진%'  → 진 앞뒤로 뭐가 와도 됨  → 최진성, 김진수, 박진아 모두 해당
'진%'   → 진으로 시작하는 것만      → 진성, 진우 해당 / 최진성 해당 안 됨
'%진'   → 진으로 끝나는 것만        → 최진, 박진 해당
```

---

## 6. ORDER BY — 정렬

엑셀에서 열 기준으로 오름차순/내림차순 정렬하는 것과 같습니다.

```sql
-- 나이 오름차순 (어린 순)
SELECT * FROM students
ORDER BY age ASC;

-- 나이 내림차순 (많은 순)
SELECT * FROM students
ORDER BY age DESC;
```

- **ASC** — 오름차순 (Ascending, A → Z, 작은 수 → 큰 수)
- **DESC** — 내림차순 (Descending, Z → A, 큰 수 → 작은 수)

> 외우는 팁: ASC는 Ascending의 앞 세 글자. 알파벳 A가 제일 앞에 오는 것처럼 작은 값부터 나옵니다.

---

## 7. LIMIT — 개수 제한

조회 결과에서 원하는 개수만큼만 가져옵니다. 정렬이랑 같이 쓰면 "상위 N개"를 뽑을 때 유용합니다.

```sql
-- 상위 3개만
SELECT * FROM students
LIMIT 3;

-- 나이가 가장 어린 학생 2명
SELECT * FROM students
ORDER BY age ASC
LIMIT 2;
```

ORDER BY로 먼저 정렬한 다음 LIMIT으로 앞에서 잘라온다고 생각하면 됩니다.

---

## 8. SELECT 실행 순서

SQL은 작성 순서와 실제 실행 순서가 다릅니다.

```
작성 순서       실제 실행 순서
SELECT      →   1. FROM     (테이블 가져오기)
FROM        →   2. WHERE    (조건으로 필터링)
WHERE       →   3. SELECT   (컬럼 선택)
ORDER BY    →   4. ORDER BY (정렬)
LIMIT       →   5. LIMIT    (개수 자르기)
```

SELECT를 제일 먼저 쓰지만, 실제로는 제일 나중에 실행됩니다. FROM으로 테이블을 먼저 가져오고, WHERE로 걸러낸 다음에야 SELECT에서 어떤 컬럼을 보여줄지 결정합니다.

---

## 자주 하는 실수 모음

**1. 문자열에 따옴표를 빠뜨리는 경우**
```sql
WHERE major = 컴퓨터소프트웨어   -- 오류! 문자열은 따옴표로 감싸야 합니다
WHERE major = '컴퓨터소프트웨어' -- 정상
```

**2. 같다는 조건에 = 를 두 개 쓰는 경우**
```sql
WHERE age == 22  -- 오류! SQL은 = 하나만 씁니다
WHERE age = 22   -- 정상
```

**3. INSERT할 때 콤마를 빠뜨리는 경우**
```sql
INSERT INTO students (name, age, major) VALUES
('최진성', 25, '컴퓨터소프트웨어'),
('이준호', 20, '산업경영')   -- 콤마 빠짐 → 오류!
('김광은', 21, '게임VR');

-- 마지막 줄 앞에 콤마가 있어야 합니다
('이준호', 20, '산업경영'),  -- 정상
('김광은', 21, '게임VR');
```

---

> 이 글은 1주차 강의자료의 보충 설명입니다. 추가로 궁금한 부분은 QnA 게시판에 남겨주세요.