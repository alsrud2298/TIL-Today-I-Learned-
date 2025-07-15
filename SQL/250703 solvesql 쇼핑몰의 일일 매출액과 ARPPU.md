### 문제 설명
- 주문 내역 데이터와 각 주문의 결제 관련 데이터를 결합하여 일별로 집계된 쇼핑몰의 결제 고객 수, 매출액, ARPPU 계산
  
### 요구 사항
- 2018년 1월 1일 이후 데이터만 포함
- ARPPU(결제 고객 1인당 평균 결제 금액) = 전체 매출액 / 결제 고객 수
- 매출 날짜 기준으로 오름차순 정렬
- 매출액, ARPPU는 반올림해 소수점 둘째자리까지 출력

### 출력 컬럼
- dt : 매출 날짜 2018-01-01
- pu :  결제 고객 수
- revenue_daily : 해당 날짜의 매출액
- arppu : 결제 고객 1인 당 평균 결제 금액
  
### 의사 코드 작성
1. 각 테이블을 2018-01-01 이후 & 필요한 컬럼만 추출 후 주문 ID 기준으로 JOIN & timestamp -> YYYY-MM-DD 형태로 변환
2. 매출 날짜 기준으로 결제 고객 수, 매출액, ARPPU 집계

### 답안 코드

```sql
WITH order_data AS (
  SELECT
    order_id,
    customer_id,
    DATE_FORMAT(order_purchase_timestamp, '%Y-%m-%d') AS dt
  FROM olist_orders_dataset
  WHERE
    order_purchase_timestamp >= '2018-01-01 00:00:00'
), payments_data AS (
  SELECT
    order_id,
    payment_value
  FROM olist_order_payments_dataset
)

SELECT
  dt,
  COUNT(DISTINCT customer_id) AS pu,
  ROUND(SUM(payment_value),2) AS revenue_daily,
  ROUND(SUM(payment_value) / COUNT(DISTINCT customer_id),2) AS arppu
FROM order_data o
JOIN payments_data p ON o.order_id = p.order_id
GROUP BY
  dt
ORDER BY
  dt
```