### 문제 설명
- Wine Quality 데이터베이스
- 크리스마스를 맞아 친구에게 와인을 선물하려고 함
- 조건에 맞는 와인 목록 출력하기
  
### 요구 사항
1. 화이트 와인일 것
2. 와인 품질 점수가 7점 이상일 것
3. 밀도와 잔여 설탕이 와인 전체의 해당 성분 평균 보다 높을 것
4. 산도가 화이트 와인 전체 평균보다 낮고, 구연산 값이 화이트 와인 전체 평균 보다 높을 것
   
### 출력 컬럼
- 모든 컬럼

### 의사 코드 작성
- color = 'white'
- quality >= 7
- density, residual_sugar의 전체 평균 구하기
- 화이트와인 전체 pH, citric_acid 평균 구하기

### 답안 코드
```sql
WITH base AS (
  SELECT
    *,
    AVG(density) OVER() AS avg_density,
    AVG(residual_sugar) OVER() AS avg_residual_sugar,
    AVG(pH) OVER(PARTITION BY color) AS avg_pH,
    AVG(citric_acid) OVER(PARTITION BY color) AS avg_citric_acid
  FROM wines
)

SELECT
  color,
  fixed_acidity,
  volatile_acidity,
  citric_acid,
  residual_sugar,
  chlorides,
  free_sulfur_dioxide,
  total_sulfur_dioxide,
  density,
  pH,
  sulphates,
  alcohol,
  quality
FROM base
WHERE color = 'white'
AND quality >= 7
AND density > avg_density
AND residual_sugar > avg_residual_sugar
AND pH < avg_pH
AND citric_acid > avg_citric_acid
```