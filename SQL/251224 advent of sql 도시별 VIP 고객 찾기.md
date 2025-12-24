### 문제 설명
- 쇼핑몰 판매 데이터
- 최근 줄어든 오프라인 상점 매출을 늘려보고자 도시별 VIP를 추려 오프라인 한정 특별 프로모션을 하려고 함

### 요구 사항
- 고객별 순매출 집계(주문 금액 - 할인 금액)
- 각 도시별 최고 순매출 고객 추출
- 반품 주문은 집계에서 제회

### 출력 컬럼
- city_id: 도시 ID
- customer_id: 최고 매출 고객의 ID
- total_spent: 해당 고객의 순매출

### 의사 코드 작성
1. 반품 주문 제외 (is_returned = 0)
2. 순 매출 계산 (total_price - discount_amount)
3. city_id 기준 RANK 계산
4. rank = 1인 고객만 추출

### 답안 코드
```sql
WITH base
AS (
  SELECT
    customer_id,
    city_id,
    total_price - discount_amount AS spent
  FROM transactions
  WHERE
    is_returned = 0
), spent_data AS (
  SELECT
    customer_id,
    city_id,
    SUM(spent) AS total_spent
  FROM base
  GROUP BY 1,2
)
SELECT
  city_id,
  customer_id,
  total_spent
FROM(
  SELECT
    *,
    RANK() OVER(PARTITION BY city_id ORDER BY total_spent DESC) AS rnk
  FROM spent_data
) AS t
WHERE rnk = 1
```