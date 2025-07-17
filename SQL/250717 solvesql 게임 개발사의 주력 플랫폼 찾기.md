### 문제 설명
- 각 게임 개발사는 주력 플랫폼을 정해두고 해당 플랫폼 위주로 게임을 출시함.
- 주력 플랫폼 = 판매량이 가장 많은 플랫폼
- 각 게임 개발사의 주력 플랫폼과 해당 플랫폼의 판매량 합계 집계
### 요구 사항
- 판매량 합계가 동일한 플랫폼이 여러 개라면 모두 출력
- 판매량 = 북미 판매량 + 유럽 판매량 + 일본 판매량 + 기타 판매량

### 출력 컬럼
-  developer
-  platform
-  sales

### 의사 코드 작성
1. 각 게임 개발사 별 플랫폼 별 판매량 합계 구하기
2. 판매량 기준 RANK() 구하기
3. DENSE_RANK() = 1 인 데이터 추출하기

### 답안 코드
``` sql
WITH total_sales AS (
  SELECT
    developer_id,
    platform_id,
    SUM(sales_na + sales_eu + sales_jp + sales_other) AS total_sales
  FROM games
  GROUP BY
    developer_id,
    platform_id
), rnk AS (
  SELECT
    developer_id,
    platform_id,
    total_sales,
    RANK() OVER(PARTITION BY developer_id ORDER BY total_sales DESC) AS rnk
  FROM total_sales
  WHERE developer_id IS NOT NULL
  AND platform_id IS NOT NULL
)

SELECT
  c.name AS developer,
  p.name AS platform,
  total_sales AS sales
FROM rnk AS r
LEFT JOIN companies AS c ON r.developer_id = c.company_id
LEFT JOIN platforms AS p ON r.platform_id = p.platform_id
WHERE
  rnk = 1
```