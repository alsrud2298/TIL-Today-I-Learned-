### 문제 설명
- DVD 대여 프랜차이즈 고객 정보, 대여 정보, 영화 정보 데이터
- 최근 다양한 OTT 서비스의 흥행으로 고객이 이탈하고 있어 우수 고객 대상 크리스마스 특별 프로모션을 진행하고자 함.
- 우수 고객들의 고객 ID 조회
  
### 요구 사항
- 현재 유효 고객 중 대여 횟수가 35회 이상인 고객
- 유효 고객 : active = 1

### 출력 컬럼
- customer_id
  
### 의사 코드 작성
- customer 테이블 & rental 테이블 customer_id 기준 INNER JOIN & active = 1
- customer_id 기준 rental_id 개수 집계
- 35회 이상인 customer_id만 추출 (HAVING 절)

### 답안 코드
```sql
SELECT
  c.customer_id
FROM customer AS c
JOIN rental AS r ON c.customer_id = r.customer_id AND c.active = 1
GROUP BY c.customer_id
HAVING COUNT(DISTINCT r.rental_id) >= 35
```