### 문제 설명
- 역대 올림픽 정보 데이터
- 1986년 ~ 2016년 개최 정보 및 참여 선수 정보

### 요구 사항
- 올림픽에 연달아 참여한 여자 배구 선수가 알고싶어요.
- 2016년까지 대한민국 국가대표팀에 소속되어 여자 배구 종목에 두 대회 이상 연속으로 참가한 선수의 ID와 이름 출력
- event = 'Volleyball Women''s Volleyball'
- team = 'KOR'

### 출력 컬럼
- id
- name

### 의사 코드 작성
- 출전 연도 -> games 테이블 개최 연도
- 종목 이름 -> events 테이블
- 팀 이름 -> teams 테이블
- 선수 이름 -> athletes 테이블
- records 테이블 기준 LEFT JOIN 하기
- 2016년까지 데이터 추출
- LAG함수로 이전 출전 연도 집계
- 최근 출전연도 = 이전 출전연도 + 4 인지?
### 답안 코드
```sql
SELECT
  DISTINCT
  id,
  name
FROM(
  SELECT
    a.id,
    a.name,
    g.year,
    LAG(g.year,1) OVER(PARTITION BY a.id, a.name ORDER BY g.year) AS prev_year
  FROM records AS r
  LEFT JOIN games AS g ON r.game_id = g.id
  LEFT JOIN events AS e ON r.event_id = e.id
  LEFT JOIN teams AS t ON r.team_id = t.id
  LEFT JOIN athletes AS a ON r.athlete_id = a.id
  WHERE
    g.year <= 2016
  AND e.event = 'Volleyball Women''s Volleyball'
  AND t.team = 'KOR'
) AS base
WHERE year = prev_year + 4
```