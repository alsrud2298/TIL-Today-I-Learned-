### 문제 설명
- 클래식 리텐션 : 최초 방문(주문,결제 등)일로부터 N일(또는 주,월,년) 후에 재방문한 고객의 비율 집계.
  
### 요구 사항
- 1년간 추이를 살펴보기 위해 월 단위 클래식 리텐션 계산
- 첫 주문으로부터 1개월 후, 2개월 후, 11개월 후에도 주문하는 고객 수 각각 계산
- first_order_month 기준 오름차순 정렬

### 출력 컬럼
- first_order_month
- month0 ~ month11

### 의사 코드 작성
- customer_stats의 first_order_date, records의 order_date -> month로 변환 & customer_id 기준으로 JOIN
- 첫 주문월 기준으로 아래 컬럼 집계
- month 0 : 주문월 = 첫 주문월인 고객 수
- month 1 : 주문월 = 첫 주문월 + 1개월인 고객 수
- month 11 : 주문월 = 첫 주문월 + 11개월인 고객 수
  
### 답안 코드
```sql
WITH base AS (
  SELECT
    DISTINCT -- 고객이 해당 월에 2번 주문했어도, 1명으로 집계 (고객수 기준이므로)
    r.customer_id,
    DATE_FORMAT(first_order_date, '%Y-%m-01') As first_order_month,
    DATE_FORMAT(order_date, '%Y-%m-01') AS order_month
  FROM records r
  JOIN customer_stats c ON r.customer_id = c.customer_id
)

SELECT
  first_order_month,
  COUNT(CASE WHEN order_month = first_order_month THEN customer_id END) AS month0,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 1 MONTH THEN customer_id END) AS month1,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 2 MONTH THEN customer_id END) AS month2,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 3 MONTH THEN customer_id END) AS month3,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 4 MONTH THEN customer_id END) AS month4,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 5 MONTH THEN customer_id END) AS month5,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 6 MONTH THEN customer_id END) AS month6,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 7 MONTH THEN customer_id END) AS month7,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 8 MONTH THEN customer_id END) AS month8,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 9 MONTH THEN customer_id END) AS month9,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 10 MONTH THEN customer_id END) AS month10,
  COUNT(CASE WHEN order_month = first_order_month + INTERVAL 11 MONTH THEN customer_id END) AS month11
FROM base
GROUP BY
  first_order_month
ORDER BY  
  first_order_month;
```