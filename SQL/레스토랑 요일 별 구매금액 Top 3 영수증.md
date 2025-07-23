### 문제 설명
- 요일 별 큰 금액을 결제한 영수증을 찾고 싶습니다.
  
### 요구 사항
- 요일 별 결제 금액으로 Top 3를 지불한 영수증 출력
- 상위 결제 금액 영수증이 3개 이상이라면, 모두 출력

### 출력 컬럼
- day : 요일
- time : 시간대
- sex : 결제자 성별
- total_bill : 식사 금액

### 의사 코드 작성
- RANK 함수 사용해서 요일별 식사 금액 기준 순위 출력
- 순위 <= 3 인 데이터만 출력하기

### 답안 코드
```sql
SELECT
  day,
  time,
  sex,
  total_bill
FROM(
  SELECT
    day,
    time,
    sex,
    total_bill,
    RANK() OVER(PARTITION BY day ORDER BY total_bill DESC) AS rnk
  FROM tips
) AS t
WHERE rnk <= 3
```