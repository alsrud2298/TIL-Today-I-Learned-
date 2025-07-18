### 문제 설명
- 이 쇼핑몰은 상품마다, 시기마다 할인율을 계속 조정합니다.
- 80% 이상 할인하는 상품은 재고 처리를 위해 판매하는 상품입니다.
- 이런 상품이 가장 많이 팔린 날이 언제인지 궁금합니다.

### 요구 사항
- 일 별로 전체 상품 판매 개수, 80% 이상 할인 상품의 판매 개수를 계산해주세요.
- 주문 날짜 기준으로 전체 상품 판매 개수가 10개 이상, 80% 이상 할인 판매한 상품이 적어도 1개인 날만 출력합니다.
- 80% 이상 할인 상품이 많이 판매된 날짜부터 보여주도록 정렬해주세요.

### 출력 컬럼
- order_date
- big_discount_items
- all_items

### 의사 코드 작성
1. all_items = order_date 별 SUM(quantity)
2. big_discount_items = SUM(IF(discount >= 0.8, quantity, 0))
3. all_items >= 10 AND big_discount_items >= 1 인 데이터만 출력
4. big_discount_items 기준 내림차순 정렬

### 답안 코드
``` sql
SELECT
  *
FROM(
  SELECT
    order_date,
    SUM(IF(discount >= 0.8, quantity, 0)) AS big_discount_items,
    SUM(quantity) AS all_items
  FROM records
  GROUP BY
    order_date
) AS t
WHERE all_items >= 10
AND big_discount_items >= 1
ORDER BY
  big_discount_items DESC
```