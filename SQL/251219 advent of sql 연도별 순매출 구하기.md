### 문제 설명
- 쇼핑몰 프랜차이즈 데이터
- 2018년부터 2023년까지 11월, 12월 상품 판매 기록
- 겨울 매출이 몇 년째 부진하여 경영진은 겨울 매출을 데이터 기반으로 개선하고자 함.
- 다양한 시즌 이벤트와 할인을 제공하고 있어 전체 매출과 반품, 할인 등을 제외한 순매출 값은 차이가 날 수 있음.
  
### 요구 사항
- 연도별 순매출 조회
- 반품되지 않은 거래 내역에 대해 주문 금액에서 할인 금액을 제외한 실제 결제 금액의 합
- 연도 기준 오름차순 정렬

### 출력 컬럼
- year: 연도
- net_sales: 해당 연도의 순매출 합

### 의사 코드 작성
1. purchased_at, total_price, discount_amount, is_returned
2. 연도별 is_returned = 0 인 거래 기록들의 total_price - discount_amount의 합 계산

### 답안 코드
```sql
SELECT
  YEAR(purchased_at) AS year,
  SUM(total_price - discount_amount) AS net_sales
FROM transactions
WHERE is_returned = 0
GROUP BY 1
ORDER BY 1
```