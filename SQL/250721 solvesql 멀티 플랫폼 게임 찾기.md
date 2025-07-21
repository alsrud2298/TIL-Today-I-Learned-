### 문제 설명
- 1980년부터 2016년까지 발매된 게임 정보 데이터
- 제작사가 같은 플랫폼 = 같은 계열 플랫폼으로 분류
- 점유율이 높은 메이저 플랫폼 3사
- Sony: 'PS3', 'PS4', 'PSP', 'PSV'
- Nintendo: 'Wii', 'WiiU', 'DS', '3DS'
- Microsoft: 'X360', 'XONE'

### 요구 사항
- 2012년 이후 출시된 게임들 중 둘 이상의 메이저 플랫폼 계열에 출시된 게임 이름 출력
- 중복된 게임은 1번만 출력되어야 함

### 출력 컬럼
- name

### 의사 코드 작성
1. platforms 테이블에서 메이저 계열사 열 추가하기
2. games 테이블과 platform_with_names 테이블 platform_id 기준으로 JOIN & platform_type이 Sony, Nintendo, Microsoft & 출시년도가 2012년 이후인 데이터만 추출
3. platform_name 개수 집계 -> 2개 이상인 게임 이름만 추출하기

### 답안 코드
``` sql
WITH platform_names AS (
  SELECT
    platform_id,
    name,
    CASE
      WHEN name IN ('PS3', 'PS4', 'PSP', 'PSV') THEN 'Sony'
      WHEN name IN ('Wii', 'WiiU', 'DS', '3DS') THEN 'Nintendo'
      WHEN name IN ('X360', 'XONE') THEN 'Microsoft'
    END AS platform_type
  FROM platforms
)
SELECT
  DISTINCT
  name
FROM(
  SELECT
    g.name,
    COUNT(DISTINCT platform_type) AS platform_cnt
  FROM games g
  LEFT JOIN platform_names p
  ON g.platform_id = p.platform_id
  WHERE year >= 2012 AND platform_type IS NOT NULL
  GROUP BY name
) AS t 
WHERE platform_cnt >= 2
```