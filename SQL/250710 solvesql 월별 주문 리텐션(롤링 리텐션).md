### 문제 설명
- 롤링 리텐션 : 최초 활성화 시점부터 특정 시점까지 아직 이탈하지 않은 고객의 비율 계산.
  - 1월에 활성화 & 3월에 서비스 다시 사용한 경우, 2월에도 '아직 이탈하지 않은 고객'으로 간주하여 계산에 포함
  - 이탈에 초점이 맞춰져 있는 리텐션
- 주문이 지속적으로 잘 일어나고 있는지 확인하기 위해, 고객의 첫 주문일을 기준으로 이후에 주문을 했는지를 바탕으로 롤링 리텐션 계산
  
### 요구 사항
- 2020년 1월에 첫 주문 -> "2020-01-01"로 첫 주문 월 표기
- first_order_month 기준 오름차순 정렬

### 출력 컬럼
- first_order_month,
- month0,
- month1, ... month11 : 해당 월에 처음 주문하고, N개월 이후에도 이탈하지 않은 고객의 수

### 의사 코드 작성
1. 고객별 첫 주문 월 & 주문 월 추출하기
2. month0 : 첫 주문 월 = 주문 월
3. month1 : 주문 월 >= 첫 주문 월 + 1개월
4. month2 : 주문 월 >= 첫 주문 월 + 2개월
5. month11까지 동일

### 답안 코드
```sql
WITH first_order AS (
  SELECT 
    customer_id,
    DATE_FORMAT(first_order_date, "%Y-%m-01") AS first_order_month
  FROM customer_stats
), records_with_first_order_month AS (
  SELECT
    r.customer_id,
    DATE_FORMAT(r.order_date, "%Y-%m-01") AS order_month,
    f.first_order_month
  FROM records r
  LEFT JOIN first_order f ON r.customer_id = f.customer_id
)

SELECT 
  first_order_month,
  COUNT(DISTINCT customer_id) AS month0,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 1 MONTH THEN customer_id END) AS month1,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 2 MONTH THEN customer_id END) AS month2,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 3 MONTH THEN customer_id END) AS month3,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 4 MONTH THEN customer_id END) AS month4,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 5 MONTH THEN customer_id END) AS month5,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 6 MONTH THEN customer_id END) AS month6,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 7 MONTH THEN customer_id END) AS month7,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 8 MONTH THEN customer_id END) AS month8,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 9 MONTH THEN customer_id END) AS month9,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 10 MONTH THEN customer_id END) AS month10,
  COUNT(DISTINCT CASE WHEN order_month >= first_order_month + INTERVAL 11 MONTH THEN customer_id END) AS month11
FROM records_with_first_order_month
GROUP BY
  first_order_month
ORDER BY
  first_order_month;
```