### 문제 설명
- 미국 온라인 쇼핑몰 2020년 주문 데이터
- 쇼핑몰 운영팀은 어떤 상품 카테고리가 신규 고객의 첫 구매를 가장 강력하게 유도하는지 파악하여 마케팅 전략에 활용하려고 합니다.

### 요구 사항
- 모든 카테고리 x 서브 카테고리 조합에 대해 해당 카테고리 조합에 속한 상품이 각 사용자의 첫 구매로 주문된 건수 집계
- 첫 주문 건수가 많은 순서대로 내림차순 정렬
- 여러 상품 묶어서 하나의 주문으로 구매한 경우가 있으므로 같은 order_id인 데이터가 여러개 존재할 수 있음
- 집계 단위는 상품의 수가 아닌 주문의 수임

### 출력 컬럼
- category
- sub_category
- cnt_orders

### 의사 코드 작성
1. records 테이블과 customer_stats 테이블 customer_id 기준으로 JOIN 후 주문 날짜 = 첫 주문일 인 데이터만 출력
2. 첫주문 데이터 테이블과 records 테이블 LEFT JOIN
3. 카테고리 조합 기준 DISTINCT order_id 카운트


### 답안 코드
```sql
WITH first_records AS ( -- 첫 주문 데이터만 추출
  SELECT
    category,
    sub_category,
    order_id
  FROM records AS r
  LEFT JOIN customer_stats cs ON r.customer_id = cs.customer_id
  WHERE r.order_date = cs.first_order_date
)

SELECT
  category,
  sub_category,
  COUNT(DISTINCT order_id) AS cnt_orders
FROM first_records
GROUP BY 1,2
ORDER BY 3 DESC
```