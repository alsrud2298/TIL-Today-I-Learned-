### 문제 설명
- 회사에서 신규 입사자들의 적응을 돕기 위해 멘토링 프로그램을 운영합니다.
- 아래 조건을 모두 만족하는 멘티-멘토 짝꿍 리스트를 계산하는 쿼리를 작성해주세요.
  
### 요구 사항
- 멘티는 2021년 12월 31일 기준 3개월 이내 입사한 인원 전체
- 멘토는 2021년 12월 31일 기준 재직한지 2년 이상이 된 직원들만 배정
- 최대한 다양한 분야 직원들이 교류할 수 있도록 서로 다른 부서에 속하는 직원끼리 멘토링 진행
- 매칭 가능한 멘토가 없는 경우도 모두 포함
- 멘티 ID 기준 오름차순 정렬, 멘토 ID 기준 오름차순 정렬

### 출력 컬럼
- mentee_id,
- mentee_name,
- mentor_id,
- mentor_name

### 의사 코드 작성
1. 멘티 테이블, 멘토 테이블 생성
2. 멘티 테이블 기준으로 멘토 테이블 LEFT JOIN (멘토가 없는 경우도 포함) & 멘토 부서 =/= 멘티 부서
3. 정렬하기

### 답안 코드
```sql
WITH mentees AS (
  SELECT
    employee_id AS mentee_id,
    name AS mentee_name,
    department AS mentee_dept
  FROM employees
  WHERE join_date >= '2021-12-31' - INTERVAL 3 MONTH
), mentors AS (
  SELECT
    employee_id AS mentor_id,
    name AS mentor_name,
    department AS mentor_dept
  FROM employees
  WHERE join_date <= '2021-12-31' - INTERVAL 2 YEAR
)

SELECT
  mentee_id,
  mentee_name,
  mentor_id,
  mentor_name
FROM mentees AS me
LEFT JOIN mentors AS mt ON me.mentee_dept != mt.mentor_dept
ORDER BY
  mentee_id,
  mentor_id
```