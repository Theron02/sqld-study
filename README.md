# SQLD 스터디 📚

SQLD(SQL 개발자) 자격증 취득을 목표로 하는 8주 스터디 그룹의 강의자료 · 실습 문제 저장소입니다.
MySQL 실습부터 데이터 모델링 이론, 옵티마이저까지 SQLD 시험 전 범위를 다룹니다.

---

## 진행 방식

- 총 **8주차** / **격주 진행** (약 2개월)
- **대면 · 비대면** 교차 운영
- 매 회차 지난 주 실습 문제 정답 확인 후 진행
- 강의자료는 매주 이 저장소에 업로드 (`.md` 자료 + `.pdf` / `.key` 슬라이드)

---

## 저장소 구조

```
sqld/
├── README.md          # 이 문서 (전체 커리큘럼)
├── 1주차/
│   ├── week1.md       # 강의자료 (마크다운)
│   ├── sqld-week1.pdf # 발표 슬라이드 (PDF)
│   ├── week1_answer.md # 1주차 정답 (마크다운)
│   └── sqld-week1.key # 발표 슬라이드 (Keynote)
├── 2주차/
│   ├── week2.md
│   ├── sqld-week2.pdf
│   ├── week2_answer.md # 2주차 정답 (마크다운)
│   └── sqld-week2.key
└── ...                # 8주차까지 순차 업로드 예정
```

각 주차 폴더에는 마크다운 강의자료와 발표 슬라이드(PDF / Keynote)가 함께 들어 있습니다.
바로 보려면 마크다운 자료부터 읽는 것을 추천합니다.

> **현재 업로드 현황** — [1주차](1주차/week1.md) ✅ · [2주차](2주차/week2.md) ✅ · 3~8주차 (예정)

---

## 전체 일정

| 주차 | 방식 | 주제 | 세부 내용 | 자료 |
|------|------|------|----------|------|
| 1주차 | 대면 | 환경 세팅 & 기본 문법 | MySQL 설치, Workbench 접속, SELECT, WHERE, ORDER BY, LIMIT, 비교 연산자 | [📄](1주차/week1.md) |
| 2주차 | 비대면 | JOIN & 집계 | JOIN (INNER/LEFT), GROUP BY, 집계 함수, HAVING, 서브쿼리 | [📄](2주차/week2.md) |
| 3주차 | 대면 | DDL & DML & 트랜잭션 | CREATE, ALTER, DROP, INSERT, UPDATE, DELETE, COMMIT, ROLLBACK, ACID | 예정 |
| 4주차 | 비대면 | 데이터 모델링 기초 | 엔티티, 속성, 관계, ERD, 식별자, 정규화 (1NF~3NF) | 예정 |
| 5주차 | 대면 | 관계형 데이터 모델 심화 | 1:1 / 1:N / N:M 관계, 반정규화, 슈퍼/서브타입, 조인 성능 | 예정 |
| 6주차 | 비대면 | SQL 활용 심화 | 윈도우 함수 (RANK, ROW_NUMBER, PARTITION BY), NULL 처리, CASE WHEN | 예정 |
| 7주차 | 대면 | 옵티마이저 & 인덱스 | 실행 계획, 인덱스 개념, 옵티마이저 동작 방식, 조인 방식 | 예정 |
| 8주차 | 비대면 | 최종 정리 & 모의고사 | 전 범위 핵심 요약, 기출 유형 풀이, 오답 정리 | 예정 |

---

## 시작하기 전에 (실습 환경)

1주차부터 MySQL 실습을 진행합니다. 첫 모임 전에 아래 환경을 준비해 주세요.

- **MySQL Server** 8.0 이상
- **MySQL Workbench** (쿼리 실행 도구)

설치 방법과 접속 설정은 [1주차 강의자료](1주차/week1.md)에 OS별로 정리되어 있습니다.

```bash
# Mac (Homebrew)
brew install mysql
brew services start mysql
```

Windows는 [MySQL Installer](https://dev.mysql.com/downloads/installer/)의 `Developer Default` 옵션으로 설치하면 Workbench까지 함께 설치됩니다.

---

## 주차별 상세 내용

### 1주차 — 환경 세팅 & 기본 문법 `대면`

**목표**
- MySQL Workbench 설치 및 접속 환경 구성
- 기본 SELECT 문법 익히기

**주요 내용**
- MySQL / Workbench 설치 (Windows / Mac)
- 데이터베이스, 테이블, 행, 열 개념
- `CREATE DATABASE` / `CREATE TABLE` / `INSERT`
- `SELECT` / `WHERE` / `ORDER BY` / `LIMIT`
- 비교 연산자 (`=`, `!=`, `BETWEEN`, `IN`, `LIKE`)

**실습 문제** 4문항 · [강의자료 보기](1주차/week1.md)

---

### 2주차 — JOIN & 집계 `비대면`

**목표**
- 두 테이블을 연결해서 조회하기
- 데이터 그룹화 및 집계

**주요 내용**
- `INNER JOIN` / `LEFT JOIN` / `RIGHT JOIN`
- 집계 함수 (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN`)
- `GROUP BY` / `HAVING`
- 서브쿼리 (WHERE절, FROM절 인라인 뷰)
- SQL 실행 순서 정리

**실습 문제** 6문항 · [강의자료 보기](2주차/week2.md)

---

### 3주차 — DDL & DML & 트랜잭션 `대면`

**목표**
- 테이블 구조 정의 및 수정
- 데이터 조작 문법 완성
- 트랜잭션 개념 이해

**주요 내용**
- DDL: `CREATE` / `ALTER` / `DROP` / `TRUNCATE`
- DML: `INSERT` / `UPDATE` / `DELETE`
- 트랜잭션: `COMMIT` / `ROLLBACK` / `SAVEPOINT`
- ACID 속성 (원자성, 일관성, 격리성, 지속성)
- 제약조건 (`PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, `UNIQUE`, `DEFAULT`)

**실습 문제** 6문항

---

### 4주차 — 데이터 모델링 기초 `비대면`

**목표**
- SQLD 이론 파트 핵심 개념 정리
- ERD 읽고 그리기

**주요 내용**
- 엔티티 / 속성 / 관계 개념
- ERD (Entity-Relationship Diagram) 작성법
- 식별자 (주식별자, 외래식별자, 복합식별자)
- 정규화: 1NF / 2NF / 3NF / BCNF
- 이상현상 (삽입 / 삭제 / 수정 이상)

**실습 문제** 5문항 (ERD 작성 포함)

---

### 5주차 — 관계형 데이터 모델 심화 `대면`

**목표**
- 테이블 간 관계 유형 완전 이해
- 반정규화와 성능 개선

**주요 내용**
- 관계 유형: 1:1 / 1:N / N:M
- N:M 관계 → 중간 테이블로 분해
- 반정규화 개념 및 적용 시점
- 슈퍼타입 / 서브타입 모델링
- 조인 성능과 인덱스의 관계

**실습 문제** 5문항

---

### 6주차 — SQL 활용 심화 `비대면`

**목표**
- 실전 쿼리 작성 능력 향상
- 윈도우 함수와 조건식 완전 정복

**주요 내용**
- 윈도우 함수: `RANK()` / `DENSE_RANK()` / `ROW_NUMBER()`
- `PARTITION BY` / `ORDER BY` (윈도우 함수 내)
- `CASE WHEN THEN ELSE END`
- NULL 처리: `IS NULL` / `IS NOT NULL` / `COALESCE` / `IFNULL` / `NVL`
- 집합 연산자: `UNION` / `UNION ALL` / `INTERSECT` / `MINUS`

**실습 문제** 6문항

---

### 7주차 — 옵티마이저 & 인덱스 `대면`

**목표**
- 쿼리 성능 이해
- 실행 계획 읽기

**주요 내용**
- 옵티마이저 개념 (규칙 기반 / 비용 기반)
- 인덱스 개념 및 종류 (B-Tree, 클러스터드, 비클러스터드)
- 인덱스가 사용되는 조건 / 사용되지 않는 조건
- 실행 계획 (`EXPLAIN`) 읽기
- 조인 방식 (Nested Loop / Sort Merge / Hash Join)

**실습 문제** 4문항

---

### 8주차 — 최종 정리 & 모의고사 `비대면`

**목표**
- 전 범위 핵심 요약 복습
- 실전 감각 익히기

**주요 내용**
- 1~7주차 핵심 개념 요약 정리
- SQLD 기출 유형 문제 풀이
- 오답 분석 및 취약점 보완
- 시험 전략 공유 (문제 유형별 접근법, 시간 배분)

**모의고사** 40문항 (실제 시험과 동일 유형)

---

## 학습 가이드

- **예습** — 모임 전 해당 주차 강의자료(`.md`)를 한 번 읽어옵니다.
- **실습** — 자료의 SQL 예제를 직접 Workbench에서 실행해 봅니다.
- **복습** — 실습 문제를 풀고, 다음 주차 시작 시 함께 정답을 확인합니다.
- **질문** — 막히는 부분은 블로그 QnA 게시판에 남겨주세요.

---

## 참고 자료

- 각 주차 강의자료는 [j-log.vercel.app](https://j-log.vercel.app) 블로그에도 업로드됩니다.
- 실습 문제 정답은 다음 주차 시작 시 함께 확인합니다.
- 질문은 블로그 QnA 게시판을 이용해 주세요.
