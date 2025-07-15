### 문제 설명
- 아래와 같이 세션 세분화해 집계하려고 함.
- 입문반 페이지를 본 세션
  - 입문반 페이지 스크롤 한 세션
  - 입문반 페이지 스크롤 하지 않은 세션
- 입문반 페이지를 안 본 세션

### 요구 사항
- page_title = "백문이불여일타 SQL 캠프 입문반" & event_name = "page_view"
- page_title = "백문이불여일타 SQL 캠프 입문반" & event_name = "scroll"

### 출력 컬럼
- total
- pv_no
- pv_yes_scroll_no
- pv_yes_scroll_yes
  
### 의사 코드 작성
- page_view 세션 데이터와 scroll 세션 데이터 추출하기
- ga 데이터와 page_view, scroll LEFT JOIN
  - page_view 시점, scroll 시점 JOIN 조건에 추가
- pv_no = total - pv_yes
- pv_yes_scroll_no = pv_yes - scroll_yes
- pv_yes_scroll_yes = scroll_yes

### 답안 코드
```sql
WITH page_view AS (
  SELECT
    DISTINCT
    user_pseudo_id,
    ga_session_id,
    event_timestamp_kst AS pv_at
  FROM ga
  WHERE
    page_title = "백문이불여일타 SQL 캠프 입문반"
  AND event_name = "page_view"
), scroll AS (
  SELECT
    DISTINCT
    user_pseudo_id,
    ga_session_id,
    event_timestamp_kst AS scroll_at
  FROM ga
  WHERE
    page_title = "백문이불여일타 SQL 캠프 입문반"
  AND event_name = "scroll"
)

SELECT
  COUNT(DISTINCT g.user_pseudo_id, g.ga_session_id) AS total,
  COUNT(DISTINCT g.user_pseudo_id, g.ga_session_id) - COUNT(DISTINCT pv.user_pseudo_id, pv.ga_session_id) AS pv_no,
  COUNT(DISTINCT pv.user_pseudo_id, pv.ga_session_id) - COUNT(DISTINCT sc.user_pseudo_id, sc.ga_session_id) AS pv_yes_scroll_no,
  COUNT(DISTINCT sc.user_pseudo_id, sc.ga_session_id) AS pv_yes_scroll_yes
FROM ga g
LEFT JOIN page_view pv ON g.user_pseudo_id = pv.user_pseudo_id AND g.ga_session_id = pv.ga_session_id
LEFT JOIN scroll sc ON g.user_pseudo_id = sc.user_pseudo_id AND g.ga_session_id = sc.ga_session_id
  AND pv.pv_at <= sc.scroll_at

```