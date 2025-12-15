### 문제 설명
- 아마존 베스트 세러 데이터
- 북클럽을 운영함
- 화제성과 작품성을 모두 잡은 소설 작가를 찾아 이달의 작가로 선정하고 해당 작가의 작품들을 리뷰하는 시간을 갖고자 함
  
### 요구 사항
- **소설**작가 !!! 
- 상위 50위 이내 작품, 연도에 상관 없이 2회 이상 등재
- 해당 작가 작품들의 평균 사용자 평점 4.5점 이상
- 평균 리뷰 수가 소설 분야 작품들의 평균 리뷰 수 이상
- 위 기준에 맞는 작가 이름 출력
- 사전순 정렬

### 출력 컬럼
- author

### 의사 코드 작성
1. 작가 기준 등재 횟수 집계
2. 작가 기준 평균 user_rating 집계
3. genre = Fiction인 작품들의 평균 리뷰 수 집계
4. 조건에 맞는 작가 이름 출력

### 답안 코드
```sql

WITH author_avg AS (
  SELECT
    author,
    COUNT(*) AS best_cnt,
    AVG(user_rating) AS avg_user_rating,
    AVG(reviews) AS avg_reviews
  FROM books
  WHERE genre = 'Fiction'
  GROUP BY 1
), genre_avg AS (
  SELECT
    genre,
    AVG(reviews) AS gen_avg_reviews
  FROM books
  WHERE genre = 'Fiction'
  GROUP BY 1
)

SELECT
  author
FROM author_avg, genre_avg
WHERE best_cnt >= 2
AND avg_user_rating >= 4.5
AND avg_reviews >= gen_avg_reviews 
ORDER BY 1
```     