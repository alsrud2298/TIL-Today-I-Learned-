### 문제 설명
- 레스토랑 영수증 데이터
- 고객 이벤트로 스탬프 제도를 운영하려고 함.
- 이벤트 시작 전, 과거 영수증 데이터 기반으로 스태프가 얼마나 발급될지 미리 확인해보려고 함.

### 요구 사항
- 25달러 이상, 스탬프 2개
- 15달러 이상, 스탬프 1개
- 그 외는 스탬프 X
- 스탬프 개수 별 영수증 개수 집계
- 스탬프 개수 오름차순
  
### 출력 컬럼
- stamp : 스탬프 개수
- count_bill : 영수증 개수
  
### 의사 코드 작성
- 영수 금액에 따른 스탬프 개수 컬럼 생성
- 스탬프 개수 별 영수증 개수 집계
- 정렬

### 답안 코드
```sql
SELECT
  stamp,
  COUNT(total_bill) AS count_bill
FROM(
  SELECT
    total_bill,
    CASE
      WHEN total_bill >= 25 THEN 2
      WHEN total_bill >= 15 THEN 1
      ELSE 0
    END AS stamp
  FROM tips
) AS stamps
GROUP BY stamp
ORDER BY stamp
```