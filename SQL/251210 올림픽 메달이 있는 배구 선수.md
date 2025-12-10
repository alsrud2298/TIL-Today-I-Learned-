### 문제 설명
- 올림픽 데이터
  
### 요구 사항
- 역대 대한민국 여자배고 대표팀이 메달을 딴 적 있을까?
- 2016년까지 대한민국 국가대표팀에 소속되어 여자 배구 종목에 참가한 선수 중 메달을 딴 선수 정보 출력
- 한 선수가 메달 여러 개 가진 경우, 쉼표로 붙여 하나의 컬럼에 출력되도록
  
### 출력 컬럼
- id
- name
- medals

### 의사 코드 작성
- 필요한 데이터 JOIN
- medal IS NOT NULL
- GROUP_CONCAT(medal SEPARATOR ',')

### 답안 코드
```sql
  SELECT
    a.id,
    a.name,
    GROUP_CONCAT(r.medal SEPARATOR ',') AS medals
  FROM records AS r
  LEFT JOIN games AS g ON r.game_id = g.id
  LEFT JOIN events AS e ON r.event_id = e.id
  LEFT JOIN teams AS t ON r.team_id = t.id
  LEFT JOIN athletes AS a ON r.athlete_id = a.id
  WHERE
    g.year <= 2016
  AND e.event = 'Volleyball Women''s Volleyball'
  AND t.team = 'KOR'
  AND medal IS NOT NULL
  GROUP BY 1,2
```