### 문제 설명
- 서울숲 일별 평균 대기오염도 데이터
- 미세먼지 수치가 지속적으로 나빠지는 경우 추출
- 해당 일자 부근 서울숲 대기 상세 분석하고자 함

### 요구 사항
- 2022년 중 이틀 연속 미세먼지 수치가 30ug/m^3 이상이 된 날 추출
- 날짜 기준 오름차순
    
### 출력 컬럼
- date_alert

### 의사 코드 작성
- 2일 연속 미세먼지 수치 증가 & 3일째 30ug/m^3 이라면 3일차 날짜 추출?
- LAG(,1), LAG(,2) 활용해 전날, 전전날 대비 미세먼지 수치가 증가했는지 체크
- 위의 조건을 만족하면서 현재의 미세먼지 수치가 30이상인 날짜만 추출
  
### 답안 코드
```sql
SELECT
  measured_at AS date_alert
FROM(
  SELECT
    measured_at,
    pm10,
    LAG(pm10,1) OVER(ORDER BY measured_at) AS prev_pm10,
    LAG(pm10,2) OVER(ORDER BY measured_at) AS prev_prev_pm10
  FROM measurements
) AS p
WHERE prev_pm10 > prev_prev_pm10
AND pm10 > prev_pm10
AND pm10 >= 30

```