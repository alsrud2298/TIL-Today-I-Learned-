### 문제 설명
- 모마 작품 데이터베이스

### 요구 사항
- 모마가 소장하고 있는 한국 감독 영화 찾기

### 출력 컬럼
- artist
- title

### 의사 코드 작성
1. artworks 테이블 classification LIKE 'Film%'
2. artwork_id = artworks_artists.artowr_id
3. artist_id = artists.artist_id
4. nationality = 'Korea'
5. name 출력
   
### 답안 코드
```sql
SELECT
  a.name AS artist,
  title
FROM artworks AS ar
LEFT JOIN artworks_artists AS aa ON ar.artwork_id = aa.artwork_id
LEFT JOIN artists AS a ON aa.artist_id = a.artist_id
WHERE a.nationality = 'Korean'
AND classification LIKE 'Film%'
```