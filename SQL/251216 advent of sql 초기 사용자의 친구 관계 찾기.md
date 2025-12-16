### 문제 설명
- 페이스북 사용자 메타정보와 친구 관계 데이터
- ID 번호가 낮을 수록 일찍 가입한 사용자임
- 서로 친구인 두 ID를 더하면 친구 관계가 발생한 시점을 간접적으로 추측할 수 있음
  - 4번 + 10번 < 27번 + 68번
- 초창기 페이스북 사용자 친구 관계 형성 분석해 전체 친구 관계 중 초기 0.1%의 친구 관게만 추출하고자 함.

### 요구 사항
- 두 사용자 ID의 합이 상위 0.1%에 들어오는 모든 친구 관계 출력
- 상위 %의 정의는 해당 친구 관계의 순위 / 전체 친구 관계의 수로 정의

### 출력 컬럼
- user_a_id: 친구 관계인 사용자 ID (A)
- user_b_id: 친구 관계인 사용자 ID (B)
- id_sum: 두 사용자 ID의 합
  
### 의사 코드 작성
- 사용자 ID 합, 전체 친구 관계 수 집계
- 사용자 ID에 따른 순위 집계
- 상위 0.1%만 추출
  - 상위 0.1% = 0.001 !! 

### 답안 코드
```sql
WITH rnk_data AS (
  SELECT
    *,
    RANK() OVER(ORDER BY id_sum) AS rnk
  FROM(
    SELECT
      user_a_id,
      user_b_id,
      user_a_id + user_b_id AS id_sum
    FROM edges
  ) AS id_sums
)

SELECT
  user_a_id,
  user_b_id,
  id_sum
FROM rnk_data
WHERE rnk <=
  (SELECT
    COUNT(DISTINCT user_a_id, user_b_id) * 0.001 AS top_10_pct
  FROM edges)

```
