### 문제 설명
- 쇼핑몰 판매 기록 데이터
- 커머스 서비스에서는 사용자 기준으로 버킷 나눌 시, 사전에 버킷이 균등하게 나뉘어져 있는지 확인하는 절차가 필요함
- 특정 버킷에 큰 손 고객이 몰려 있는 경우 테스트 결과가 왜곡 될 수 있기 때문

### 요구 사항
- 버킷별 사용자 수, 평균 주문 수, 평균 주문 금액 확인
- 반품된 주문 제외
- 둘째자리 반올림

### 출력 컬럼
- bucket: 사용자 버킷
- user_count: 버킷에 포함된 사용자 수
- avg_orders: 버킷에 포함된 사용자들의 평균 주문 수
- avg_revenue: 버킷에 포함된 사용자들의 평균 주문 금액 ($)

### 의사 코드 작성
- 반품 주문 제외 테이블 생성
- 사용자 ID 기준 %10 = 0이면 A, 그 외에는 B
- 버킷별 사용자 수, 평균 주문 수, 평균 주문 금액 집계

### 답안 코드
```sql
WITH base AS (
  SELECT
    customer_id,
    CASE
      WHEN customer_id % 10 = 0 THEN 'A'
      ELSE 'B'
    END AS bucket,
    COUNT(transaction_id) AS order_count,
    SUM(total_price) AS total_revenue
  FROM transactions
  WHERE is_returned = 0
  GROUP BY customer_id
)


SELECT
  bucket,
  COUNT(DISTINCT customer_id) AS user_count,
  ROUND(AVG(order_count),2) AS avg_orders,
  ROUND(AVG(total_revenue),2) AS avg_revenue
FROM base AS b
GROUP BY bucket
```