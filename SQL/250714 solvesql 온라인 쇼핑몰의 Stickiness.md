### 문제 설명
- 고객들의 재방문 정도를 확인하기 위해 Stickiness(고객 고착도)를 확인하려고 합니다.
- 일반적으로 Stickiness = DAU / WAU
- 온라인 쇼핑몰에서는 주문 완료 액션을 중요하게 생각하기 때문에 해당 기간 동안 한 번이라도 주문한 고객을 '활성 고객'으로 정의합니다.

### 요구 사항
- 2020년 11월 한 달 동안의 일별 DAU, WAU, Stickiness 계산
- stickiness는 반올림하여 소수점 둘째자리까지만 출력
- 한번도 주문하지 않은 날은 집계에서 제외
- dt 기준 오름차순 정렬

### 출력 컬럼
- dt
- dau : 기준 날짜에 주문한 고객 수
- wau : 기준 날짜 6일 전부터 해당 날짜까지 상품을 주문한 고객 수
- stickiness

### 의사 코드 작성
1. 2020년 11월 1일 ~ 11월 30일까지의 데이터 추출
2. DAU = COUNT(DISTINCT customer_id) 계산
3. dau dt 기준으로 6일 전 주문 데이터와 JOIN (wau 계산용)
   1. dau 테이블과 records 테이블 LEFT JOIN ON r.order_date BETWEEN d.dt - INTERVAL 6 DAY AND d.dt , WHERE dau > 0 
4. wau, stickiness 계산

  | JOIN 없이 WINDOW 함수로 계산 시, 11월 1일의 wau가 1이 됨.

### 답안 코드
``` sql
WITH base AS (
  SELECT
    DISTINCT -- 해당 날짜에 한 번이라도 주문한 경우 활성 고객
    order_date AS dt,
    customer_id
  FROM records
  WHERE order_date BETWEEN '2020-11-01' AND '2020-11-30'
), dau AS (
  SELECT
    dt,
    COUNT(DISTINCT customer_id) AS dau
  FROM base
  GROUP BY dt
)

SELECT
  dt,
  dau,
  COUNT(DISTINCT customer_id) AS wau,
  ROUND(dau / COUNT(DISTINCT customer_id),2) AS stickiness
FROM dau d
LEFT JOIN records r ON r.order_date BETWEEN d.dt - INTERVAL 6 DAY AND d.dt
WHERE dau != 0
GROUP BY
  dt, dau
ORDER BY 
  dt
```