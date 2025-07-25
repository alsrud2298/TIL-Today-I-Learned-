### 문제 설명
- 우리 플랫폼에 정착한 판매자 id와 주문 건수 출력하기

### 요구 사항
- 상품 가격이 50달러 이상인 주문이 100건 이상 들어온 판매자 리스트
- 주문 건수가 많은 순서대로 출력

### 출력 컬럼
- seller_id
- orders

### 의사 코드 작성
- 상품 가격이 50달러 이상인 주문 데이터만 추출
- 판매자 id 기준으로 주문 건수 집계 -> 100건 이상인 데이터만 필터링
- 주문 건수 기준 정렬하기

### 답안 코드
```sql
SELECT
  seller_id,
  COUNT(DISTINCT order_id) AS orders
FROM olist_order_items_dataset
WHERE price >= 50
GROUP BY
  seller_id
HAVING
  COUNT(DISTINCT order_id) >= 100
ORDER BY
  orders DESC
```