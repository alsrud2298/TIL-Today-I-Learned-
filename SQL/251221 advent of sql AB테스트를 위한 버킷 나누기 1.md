### 문제 설명
- 쇼핑몰 겨울 판매 데이터
- 온라인 쇼핑몰 지표 개선을 위해 웹사이트 디자인 개편을 준비하고 있음
- 새로운 디자인이 지표 개선을 이끄는지 확인하고자 함
- 신규 디자인을 전체 10% 사용자에게만 먼저 새로운 디자인 적용하는 A/B 테스트 진행하기로 함

### 요구 사항
- 고객 ID가 연속된 정수
- 고객 ID 기준 10으로 나눈 나머지가 0이면 그룹 A
- 나머지 사용자를 그룹 B
- A그룹만 신규 디자인을 보도록 작업함
- 전체 고객별 할당된 그룹을 구하는 쿼리 작성
- 고객 ID 기준 오름차순

### 출력 컬럼
- customer_id
- bucket

### 의사 코드 작성
- CASE WHEN 사용

### 답안 코드
```sql
SELECT
  DISTINCT
  customer_id,
  CASE
    WHEN customer_id % 10 = 0 THEN 'A'
    ELSE 'B'
  END AS bucket
FROM transactions
ORDER BY 1
```