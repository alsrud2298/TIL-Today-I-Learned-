### 문제 설명
- OTT 플랫폼 서비스 영화 정보 데이터
- 크리스마스에 연인과 함께 볼 사랑에 대한 영화를 찾고 싶습니다.

### 요구 사항
- 영화 제목에 Love 또는 love 라는 글자가 포함된 영화 찾기
- 평점 내림차순, 개봉 연도 내림차순

### 출력 컬럼
- title
- year
- rotten_tomatoes
  
### 의사 코드 작성
- WHERE 절에서 조건 필터링하기
- 정렬

### 답안 코드

```sql

SELECT 
  title,
  year,
  rotten_tomatoes
FROM movies
WHERE
  title LIKE '%Love%' OR title LIKE '%love%'
ORDER BY  
  rotten_tomatoes DESC,
  year DESC
```