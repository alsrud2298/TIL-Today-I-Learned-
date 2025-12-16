### 문제 설명
- DVD 대여 프랜차이즈 데이터
- 대여 매출이 높았던 상위 5명의 배우 추려서 특별 코너 만들어 프로모션에 활용하려고 함

### 요구 사항
- 배우별 대여 매출 합계 계산
  - 배우가 출연한 작품들의 매출 합계
- 상위 5명 출력
- 매출액 기준 내림차순 정렬

### 출력 컬럼
- first_name
- last_name
- total_revenue

### 의사 코드 작성
-- 필요한 데이터를 먼저 가져오자 !! 
1. actor -> film_actor -> inventory -> rental -> payment
2. 매출액 -> 대여기록 -> 재고id
   
### 답안 코드
```sql
SELECT
  first_name,
  last_name,
  SUM(amount) As total_revenue 
FROM actor AS a
JOIN film_actor AS fa ON a.actor_id = fa.actor_id
JOIN inventory AS i on fa.film_id = i.film_id
JOIN rental AS r ON i.inventory_id = r.inventory_id
JOIN payment AS p ON r.rental_id = p.rental_id
GROUP BY a.actor_id, 1,2 -- 동명이인이 있을 수 있으므로 actor_id로도 그룹화!!
ORDER BY 3 DESC
LIMIT 5
```