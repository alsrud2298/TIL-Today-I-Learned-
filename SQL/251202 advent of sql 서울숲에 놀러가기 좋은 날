### 문제 설명
- 서울숲 일별 평균 대기 오염도 데이터
- 야외 활동하기 좋은 날을 알고 싶습니다.
- 
### 요구 사항
- 2022년 12월 중
- 초미세먼지 농도 9ug/m^3 이하

### 출력 컬럼
- good_day : 서울숲에서 야외 활동을 하기 좋은 날

### 의사 코드 작성
- 2022년 12월 & pm2_5 9이하 필터링
- 정렬

### 답안 코드
```sql
SELECT
  DISTINCT
  measured_at AS good_day
FROM measurements
WHERE
  measured_at BETWEEN '2022-12-01' AND '2022-12-31'
AND pm2_5 <= 9
ORDER BY
  1
```