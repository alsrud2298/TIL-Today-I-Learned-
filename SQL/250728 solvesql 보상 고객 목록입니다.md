### 문제 설명
- 인도 이커머스 웹사이트 판매 데이터
- 2018년 4월 1일부터 5일까지 주문된 상품의 출고가 지연되는 문제가 있었음
- 보상을 위해 해당 상품 출고 시 사은품을 함께 전달하고자 함

### 요구 사항
- 2018-04-01 ~ 2018-04-05까지 5일 간 주문된 내역의 주문 날짜, 주문 번호, 주문자 명 출력
- 주문 날짜 순 오름차순, 같은 경우 주문 번호 순 오름차순 정렬

### 출력 컬럼
- order_date
- order_id
- customer_name

### 의사 코드 작성
- 5일 간의 주문 데이터 추출 & 정렬하기

### 답안 코드
```sql
SELECT
  order_date,
  order_id,
  customer_name
FROM orders
WHERE
  order_date BETWEEN '2018-04-01' AND '2018-04-05'
ORDER BY
  order_date,
  order_id
```
