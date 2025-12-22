### 문제 설명
- 레스토랑 영수증 데이터
- 신규 직원 고용과 적절한 재료 수급을 위해 주중, 주말 각각의 매출액 규모를 파악하려고 함.
  
### 요구 사항
- 주중, 주말 합계 매출액 규모 집계
- 매출액 합계 기준 내림차순 정렬

### 출력 컬럼
- week: 토/일요일은 weekend, 평일은 weekday
- sales

### 의사 코드 작성
- day(요일) 컬럼 기준 주말/평일 구분
- week 컬럼 기준 sales 합계
- 정렬

### 답안 코드
```sql
-- 주말의 총 매출이 평일의 약 2.5배!
SELECT
  week,
  SUM(total_bill) AS sales
FROM(
  SELECT
    CASE  
      WHEN day IN ('Sun', 'Sat') THEN 'weekend'
      ELSE 'weekday'
    END AS week,
    total_bill
  FROM tips
) AS base
GROUP BY week
ORDER BY sales DESC
```