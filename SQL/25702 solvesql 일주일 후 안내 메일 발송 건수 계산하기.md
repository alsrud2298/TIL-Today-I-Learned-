### 문제 설명
- 등록이 된 날짜로부터 **일주일 후**, 동물 등록과 관련된 **주요 문의 사항과 답변을 메일로 발송하는 프로세스**를 만들려고 합니다.
- 일별로 예상되는 메일 발송 건수를 계산해주세요. 
  
### 요구 사항
- 발송 건수는 license_number를 기준으로 중복 없이 집계
- 2016년 10월 1일부터 발급된 동물인 경우에만 메일 발송 대상으로 합니다.
- 메일 발송 예정일을 기준으로 오름차순 정렬

### 출력 컬럼
- email_send_date : YYYY-MM-DD
- email_cnts

### 의사 코드 작성
1. license_issue_date >= 2016-10-01
2. 등록일 + 7일인 email_send_date 생성 DATE_ADD()
3. email_send_date 기준 license_number 집계 DISTINCT
4. 발송 예정일 기준 오름차순 정렬

### 답안 코드
```sql
SELECT
  DATE_ADD(license_issue_date, INTERVAL 7 DAY) AS email_send_date,
  COUNT(DISTINCT license_number) AS email_cnts
FROM seattle_pet_licenses
WHERE license_issue_date >= '2016-10-01'
GROUP BY
  email_send_date
ORDER BY
  email_send_date;
```