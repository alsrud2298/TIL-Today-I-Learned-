### 문제 설명
- 게임 정보 데이터
- 신규 게임 런칭의 책임자로서 고퀄리티 게임을 만들고자 계획을 세우고 있음
- 새로운 게임의 장르를 결정하려고 함

### 요구 사항
- 2011년부터 2015년까지 출시된 각 장르의 평론가 점수 평균 계산
- 평론가 점수가 없는 게임은 제외
- 소수점 아래 둘째자리까지 출력

### 출력 컬럼
- genre: 게임 장르
- score_2011: 2011년에 출시된 해당 장르 게임의 평론가 점수 평균
- score_2012: 2012년에 출시된 해당 장르 게임의 평론가 점수 평균
- score_2013: 2013년에 출시된 해당 장르 게임의 평론가 점수 평균
- score_2014: 2014년에 출시된 해당 장르 게임의 평론가 점수 평균
- score_2015: 2015년에 출시된 해당 장르 게임의 평론가 점수 평균

### 의사 코드 작성
- genre_id 기준 games 테이블과 genres 테이블 JOIN
- genre, year, critic_score 가져오기
- 장르 기준으로 년도별 피봇 AVG(CASE WHEN year = ... THEN critic_score END))
  
### 답안 코드
```sql
SELECT
  genre,
  ROUND(AVG(CASE WHEN year = 2011 THEN critic_score END),2) AS score_2011,
  ROUND(AVG(CASE WHEN year = 2012 THEN critic_score END),2) AS score_2012,
  ROUND(AVG(CASE WHEN year = 2013 THEN critic_score END),2) AS score_2013,
  ROUND(AVG(CASE WHEN year = 2014 THEN critic_score END),2) AS score_2014,
  ROUND(AVG(CASE WHEN year = 2015 THEN critic_score END),2) AS score_2015
FROM(
  SELECT
    ge.name AS genre,
    year,
    critic_score
  FROM games AS ga
  LEFT JOIN genres AS ge ON ga.genre_id = ge.genre_id
  WHERE
    year BETWEEN 2011 AND 2025
  AND critic_score IS NOT NULL
) AS base 
GROUP BY genre
```