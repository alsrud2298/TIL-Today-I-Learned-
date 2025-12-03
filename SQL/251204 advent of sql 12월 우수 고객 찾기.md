### 문제 설명
- 2020년 미국 커머스 업체 주문 정보 데이터
- 12월 판매 실적을 개선하기 위해 지난 12월 매출이 높았던 고객에게 프로모션을 제공하고자 함.

### 요구 사항
- 2020년 12월 동안 모든 주문의 매출 합계가 1000달러 이상인 고객 ID 출력
  
### 출력 컬럼
- customer_id

### 의사 코드 작성
- 2020년 12월 주문 데이터만 추출
- customer_id 별로 매출 집계
- 1000달러 이상인 고객 id만 필터링

### 답안 코드
```sql
SELECT
  customer_id
FROM records
WHERE
  order_date BETWEEN '2020-12-01' AND '2020-12-31'
GROUP BY
  customer_id
HAVING
  SUM(sales) >= 1000
```