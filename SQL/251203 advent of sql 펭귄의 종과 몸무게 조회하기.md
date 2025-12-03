### 문제 설명
- 남극 서식 펭귄의 종, 서식지, 신체 특징 데이터
- 펭귄의 종과 몸무게의 관계에 대해 알아보기 위한 기초 데이터 추출

### 요구 사항
- 펭귄의 종, 몸무게 정보가 담긴 열 출력
- 종 또는 몸무게가 없는 개체는 제외
- 몸무게 내림차순, 종 오름차순 정렬

### 출력 컬럼
- species
- body_mass_g

### 의사 코드 작성
- 종과 몸무게 모두 NULL이 아니어야 함
- 정렬

### 답안 코드
```sql
SELECT
  species,
  body_mass_g
FROM penguins
WHERE
  species IS NOT NULL
AND body_mass_g IS NOT NULL
ORDER BY
  body_mass_g DESC,
  species
```