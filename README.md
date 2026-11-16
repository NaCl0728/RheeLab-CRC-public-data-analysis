# Rhee Lab · CRC public data analysis

Analysis notebooks for public colorectal / rectal cancer datasets, by Rhee Lab.

대장암·직장암 관련 공개 데이터를 분석한 노트북 모음입니다. 각 폴더가 데이터셋 하나이며, 폴더 안 README에 데이터 출처, 분석 방법, 결과, 재현 정보가 있습니다.

## 분석 목록

| 폴더 | 데이터 | 플랫폼 | 내용 |
|---|---|---|---|
| [`GSE279942_GeoMx`](GSE279942_GeoMx/) | GSE279942 (Corrò et al., *iScience* 2026) | GeoMx WTA | nCRT 전 직장암 생검 51명, 면역글로불린 신호와 병리 반응(TRG) |

## 공통 사항
- 각 노트북은 Google Colab에서 위에서부터 차례로 실행하면 공개 저장소(GEO 등)에서 원본 데이터를 직접 받아 결과까지 재현됩니다
- 원본 데이터 파일은 저장소에 포함하지 않습니다 (권리는 원 저자에게 있음)
- 재현 정보(패키지 버전, 입력 파일 지문 등)는 각 폴더의 `results/run_info.json`에 있습니다
- 코드 작성에 AI(Claude)를 활용했고, 분석 설계와 결과 검증은 직접 수행했습니다
