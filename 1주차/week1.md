# SQLD 스터디 1주차 — MySQL 환경 세팅 & 기본 문법

## 이번 주 목표

- MySQL Workbench 설치 및 접속 환경 구성
- 데이터베이스 / 테이블 개념 이해
- SELECT 문 기본 문법으로 데이터 조회하기

---

## 1. MySQL 설치

### Windows

1. [MySQL 공식 다운로드 페이지](https://dev.mysql.com/downloads/installer/) 접속
2. `MySQL Installer for Windows` 다운로드
3. 설치 타입 선택 시 `Developer Default` 선택
4. 설치 중 root 비밀번호 설정 (꼭 기억해두기)
5. 설치 완료 후 MySQL Workbench 자동 설치됨

### Mac

```bash
brew install mysql
brew services start mysql
```

Workbench는 [공식 사이트](https://dev.mysql.com/downloads/workbench/) 에서 별도 다운로드

---

## 2. MySQL Workbench 접속하기

1. Workbench 실행
2. `+` 버튼 눌러서 새 커넥션 생성
3. 연결 정보 입력
   - Connection Name: 자유롭게 (예: `local`)
   - Hostname: `127.0.0.1`
   - Port: `3306`
   - Username: `root`
4. `Test Connection` 으로 연결 확인
5. 비밀번호 입력 후 접속

접속 화면이 뜨면 왼쪽에 `Schemas` 탭에서 데이터베이스 목록을 확인할 수 있음.

---

## 3. 데이터베이스 / 테이블 개념

| 용어 | 설명 |
|------|------|
| Database | 데이터를 저장하는 큰 단위 (엑셀 파일 하나라고 생각하면 됨) |
| Table | 데이터베이스 안의 표 (엑셀 시트 하나) |
| Row (행) | 테이블의 한 줄 — 데이터 하나 |
| Column (열) | 테이블의 세로줄 — 속성 하나 |

예시로 `students` 테이블을 떠올려보면

| id | name | age | major |
|----|------|-----|-------|
| 1  | 최진성 | 25 | 컴퓨터소프트웨어 |
| 2  | 이준호 | 20 | 컴퓨터소프트웨어 |

- 테이블 이름: `students`
- 컬럼: `id`, `name`, `age`, `major`
- 행: 학생 한 명 한 명의 데이터

---

## 4. 데이터베이스 생성 및 사용

```sql
-- 데이터베이스 생성
CREATE DATABASE sqld_study;

-- 생성한 데이터베이스 사용하기
USE sqld_study;

-- 현재 어떤 DB를 쓰고 있는지 확인
SELECT DATABASE();
```

---

## 5. 테이블 생성 및 데이터 넣기

실습용 테이블을 만들어보자.

```sql
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    age INT,
    major VARCHAR(50)
);

INSERT INTO students (name, age, major) VALUES
('최진성', 25, '컴퓨터소프트웨어'),
('이준호', 20, '산업경영'),
('김광은', 21, '게임VR'),
('정민준', 20, '시각디자인'),
('함혁주', 24, '컴퓨터전자공학'),
('이혁주', 27, '컴퓨터소프트웨어');
```

> 각 행을 콤마(`,`)로 구분하고 마지막 행만 세미콜론(`;`)으로 끝내야 합니다. 콤마를 빠뜨리면 `Error Code: 1064` 문법 오류가 발생하니 주의하세요.

---

## 6. SELECT 기본 문법

### 전체 조회

```sql
SELECT * FROM students;
```
`*` 은 모든 컬럼을 의미함.

### 특정 컬럼만 조회

```sql
SELECT name, age FROM students;
```

### 조건으로 조회 — WHERE

```sql
-- 나이가 22살 이상인 학생
SELECT * FROM students
WHERE age >= 22;

-- 전공이 컴퓨터소프트웨어인 학생
SELECT * FROM students
WHERE major = '컴퓨터소프트웨어';
```

### 정렬 — ORDER BY

```sql
-- 나이 오름차순
SELECT * FROM students
ORDER BY age ASC;

-- 나이 내림차순
SELECT * FROM students
ORDER BY age DESC;
```

### 개수 제한 — LIMIT

쿼리 결과에서 원하는 개수만큼만 가져오고 싶을 때 `LIMIT`을 씁니다.

```sql
-- 상위 3개만 조회
SELECT * FROM students
LIMIT 3;
```

`ORDER BY`와 같이 쓰면 정렬한 다음 일부만 잘라올 수 있어서 자주 함께 사용됩니다.

```sql
-- 나이순 정렬 후 가장 어린 학생 2명만 조회
SELECT * FROM students
ORDER BY age ASC
LIMIT 2;
```

정렬을 먼저 하고 그 다음에 LIMIT으로 잘라낸다는 순서로 이해하면 헷갈리지 않습니다. `ORDER BY age ASC`로 나이 적은 사람부터 줄을 세운 다음, `LIMIT 2`로 맨 앞 2명만 가져오는 거예요.

### 조합해서 쓰기

```sql
SELECT name, age, major
FROM students
WHERE major = '컴퓨터소프트웨어'
ORDER BY age DESC
LIMIT 2;
```

---

## 비교 연산자 정리

| 연산자 | 의미 |
|--------|------|
| `=` | 같다 |
| `!=` 또는 `<>` | 다르다 |
| `>` `<` | 크다 / 작다 |
| `>=` `<=` | 크거나 같다 / 작거나 같다 |
| `BETWEEN A AND B` | A와 B 사이 |
| `IN (값1, 값2, ...)` | 여러 값 중 하나 |
| `LIKE '%키워드%'` | 특정 문자 포함 |

```sql
-- 나이가 20~22살 사이
SELECT * FROM students
WHERE age BETWEEN 20 AND 22;

-- 전공이 산업경영 또는 컴퓨터전자공학
SELECT * FROM students
WHERE major IN ('산업경영', '컴퓨터전자공학');

-- 이름에 '진'이 들어가는 학생
SELECT * FROM students
WHERE name LIKE '%진%';
```

---

## 이번 주 실습 문제

1. `students` 테이블에서 나이가 21살 초과인 학생을 모두 조회하시오.
2. 전공이 `컴퓨터소프트웨어`인 학생을 이름 오름차순으로 정렬해서 조회하시오.
3. 이름에 `이`가 들어가는 학생을 조회하시오.
4. 나이순으로 정렬했을 때 가장 어린 학생 2명만 조회하시오.

---

## 다음 주 예고

- JOIN (테이블 여러 개 합치기)
- GROUP BY / 집계 함수 (COUNT, SUM, AVG)

---

> 이 글은 SQLD 스터디그룹 1주차 자료입니다. 실습하다 막히는 부분은 QnA 게시판에 남겨주세요.

***PDF 5. 테이블 생성 및 데이터 넣기에 ('함혁주', 24, '컴퓨터전자공학') 뒤에 콤마(`,`) 빠져있습니다 인지해주세용***