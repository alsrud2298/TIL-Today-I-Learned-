### 문제 설명
- 쇼핑몰 판매 데이터
- 최근 온라인 주문 증가, 기상 악화로 배송 지연이 잦아지고 있음
- 하루 배송이 중단되어 다음 날로 이월될 경우, 물류 센터가 감당해야 할 최대 물량이 어느 정도인지 시뮬레이션하고 싶음.
  
### 요구 사항
- 2023년 11월, 12월 주문에 대해 아래 데이터 추출
- 주문 건수 집계는 온라인 주문 건만 포함
- 주문일 기준 오름차순 정렬

### 출력 컬럼
- order_date: 주문일
- weekday: 요일 (Sunday, Monday, ..., Saturday)
- num_orders_today: 주문일 당일의 주문 건수
- num_orders_from_yesterday: 주문일 하루 이전 날짜부터 주문일 당일까지 연속된 이틀간의 합계 주문 건수의 합

### 의사 코드 작성
1. is_online_order = 1 & purchased_at 이 2023년 11월, 12월인 데이터만 추출
2. purchased_at -> DAYNAME 추출
3. 날짜 기준 COUNT
4. LAG() 함수로 전날 주문 건수 가져오기 + 당일 주문건수와 더하기


### 답안 코드
```sql
WITH base AS (
  SELECT
    DATE(purchased_at) AS order_date,
    DAYNAME(purchased_at) AS weekday,
    COUNT(DISTINCT transaction_id) AS num_orders_today
  FROM transactions
  WHERE is_online_order = 1
  AND purchased_at BETWEEN '2023-11-01 00:00:00' AND '2023-12-31 23:59:59'
  GROUP BY 1,2
)

SELECT
  order_date,
  weekday,
  num_orders_today,
  num_orders_today + IFNULL(LAG(num_orders_today, 1) OVER(ORDER BY order_date),0) AS num_orders_from_yesterday
FROM base
ORDER BY 1
```