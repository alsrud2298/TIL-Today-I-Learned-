### 문제 설명
- 프랜차이즈 쇼핑몰 2018-2023년까지의 11,12월 판매 기록
- 최근 온라인 쇼핑몰 판매량이 오프라인 지점들의 판매량을 추월했기에 배송 업체와 계약을 유리한 조건으로 하는 것이 실적 개선에 중요한 부분이 됨.
- 내년도 배송 업체 계약을 위해 연도별 배송 업체 이용 건수의 통계를 집계하고자 함
- 반품이 발생할 경우, 반품 회수를 위해 일반 배송 서비스 + 1회
  - DB에는 반품 거래에 대한 추가 배송 기록 없음

### 요구 사항
- 배송 업체 이용 건수를 배송 옵션별로 집계(Standard, Express, Overnight)
- 연도 기준 오름차순 정렬

### 출력 컬럼
- year
- standard
- express
- overnight

### 의사 코드 작성
- purchased_at -> year로 변환
- is_online_order = 1인 데이터만 추출
- 연도별 shipping_method 건 수 집계
  - 연도별 is_returned = 1인 건 합계 구해서 standard에 더하기
- 정렬

### 답안 코드
```sql
WITH online_orders AS (
  SELECT
    YEAR(purchased_at) AS year,
    shipping_method,
    is_returned
  FROM transactions
  WHERE is_online_order = 1
), orders_by_delivery AS (
  SELECT  
    year,
    SUM(IF(shipping_method = 'Standard',1,0)) AS standard,
    SUM(IF(shipping_method = 'Express',1,0)) AS express,
    SUM(IF(shipping_method = 'Overnight',1,0)) AS overnight,
    SUM(is_returned) AS returned_cnt
  FROM online_orders
  GROUP BY year
)

SELECT
  year,
  standard + returned_cnt AS standard,
  express,
  overnight
FROM orders_by_delivery
ORDER BY year
```