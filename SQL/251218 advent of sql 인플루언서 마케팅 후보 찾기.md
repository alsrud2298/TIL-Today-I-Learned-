### 문제 설명
- 페이스북 친구 관계 데이터
- 페이스북을 통해 인플루언서 마케팅을 하고자 함
- 비용과 마케팅 효과를 생각해 적절한 인플루언서를 선정하고자 함

### 요구 사항
- 후보의 친구 수가 100명 이상
- 후보 친구들의 친구 수 합계(a) 후보 친구 수(b) 일 때, a/b 비율이 높은 순으로 선정
  - 단, 친구들의 친구 수 합계 계산에는 중복된 친구와 후보자도 포함할 것
- 이 기준으로 인플루언서 후보 5명 추천
- 비율은 소수점 둘째자리까지, 내림차순 정렬
- edges 테이블 셀프 조인 -> 계산량이 많아서 안됨.

### 출력 컬럼
- user_id: 후보의 사용자 ID
- friends: 후보의 친구 수
- friends_of_friends: 후보 친구들의 친구 수 합계
- ratio: 후보 친구들의 친구 수 합계와 후보 친구 수의 비율

### 의사 코드 작성
1. (user_a_id, user_b_id)인 테이블과 (user_b_id, user_a_id)인 테이블 UNION (세로 병합) -> user_a_id, friends_id
2. user_a_id 기준으로 friends_id 집계
3. 친구 수 100명 이상인 user_a_id만 출력
4. user_a_id의 친구 목록 출력 -> 해당 친구들의 친구 수 합 계산
5. a/b 비율 계산하기
6. Top5 출력 

### 답안 코드
```sql
WITH friends AS (
  (SELECT
    user_a_id,
    user_b_id AS friend_id
  FROM edges)
  UNION
  (SELECT
    user_b_id,
    user_a_id AS friend_id
  FROM edges)
), friends_cnt AS(
  SELECT
    user_a_id,
    COUNT(DISTINCT friend_id) AS cnt
  FROM friends
  GROUP BY 1 
), influencers AS (
SELECT
  user_a_id,
  COUNT(DISTINCT friend_id) AS cnt
FROM friends
GROUP BY 1
HAVING cnt >= 100
), fof AS (
  SELECT
    DISTINCT
    f.user_a_id AS user_id,
    SUM(fc.cnt) AS friends_of_friends
  FROM friends AS f
  LEFT JOIN friends_cnt AS fc ON f.friend_id = fc.user_a_id
  WHERE f.user_a_id IN (SELECT user_a_id FROM influencers)
  GROUP BY user_id
)

SELECT
  fof.user_id,
  i.cnt AS friends,
  fof.friends_of_friends,
  ROUND(fof.friends_of_friends / i.cnt,2) AS ratio
FROM influencers AS i
JOIN fof ON i.user_a_id = fof.user_id
ORDER BY ratio DESC
LIMIT 5
```