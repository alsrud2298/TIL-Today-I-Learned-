### 문제 설명
- 데이터리안 웹사이트에 접속한 사용자 행동 데이터
- 전체 세션을 입문반 페이지를 본 세션과 안 본 세션으로 나누어 집계

### 요구 사항
- user_pseudo_id 와 ga_session_id 컬럼은 N:M 관계. 즉, 한 명의 사용자가 여러 개의 ga_session_id를 가질 수 있음.
- 따라서 개별 세션을 특정하려면 user_pseudo_id와 ga_session_id 컬럼을 모두 고려해야 함.

### 출력 컬럼
- total
- pv_no ( = total - pv_yes)
- pv_yes
  
### 의사 코드 작성
1. page_title = "백문이불여일타 SQL 캠프 입문반" & event_name = "page_view"인 세션 데이터 추출 (page_view_sessions)
2. 전체 세션 수 계산, pv_yes 계산 & pv_no = total - pv_yes로 계산

### 답안 코드
```sql
WITH page_view_sessions AS (
  SELECT
    DISTINCT
    user_pseudo_id,
    ga_session_id
  FROM ga
  WHERE
    page_title = "백문이불여일타 SQL 캠프 입문반"
    AND event_name = "page_view"
)

SELECT
  COUNT(DISTINCT g.user_pseudo_id, g.ga_session_id) AS total,
  COUNT(DISTINCT g.user_pseudo_id, g.ga_session_id) - COUNT(DISTINCT pvs.user_pseudo_id, pvs.ga_session_id) AS pv_no,
  COUNT(DISTINCT pvs.user_pseudo_id, pvs.ga_session_id) AS pv_yes
FROM ga g
LEFT JOIN page_view_sessions AS pvs
  ON g.user_pseudo_id = pvs.user_pseudo_id AND g.ga_session_id = pvs.ga_session_id

```