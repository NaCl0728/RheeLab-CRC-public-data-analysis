# GSE279942 re-analysis: immunoglobulin signal vs. nCRT response in rectal cancer biopsies

Re-analysis of the public GeoMx WTA dataset **GSE279942** (Corrò et al., *iScience* 2026) — pre-treatment rectal cancer biopsies from 51 patients with pathological response (TRG) to neoadjuvant chemoradiotherapy (nCRT).

nCRT 전 직장암 생검 51명의 공개 GeoMx 데이터에서 면역글로불린(IG) 신호가 병리 반응과 어떻게 연관되는지 다시 분석한 저장소입니다.

![main figure](results/Fig_GSE279942_IG_by_response.png)

## 데이터

| 항목 | 내용 |
|---|---|
| GEO | [GSE279942](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE279942) |
| 원 논문 | Corrò C, …, Tsantoulis P, Koessler T. *iScience* 2026;29:114455. [doi:10.1016/j.isci.2025.114455](https://doi.org/10.1016/j.isci.2025.114455) |
| 플랫폼 | NanoString GeoMx DSP, Human Whole Transcriptome Atlas (FFPE TMA) |
| 샘플 | nCRT 전 생검 51명, segment 97개 (PanCK+ 종양 구획 50 / PanCK− 기질 구획 47) |
| 반응 | Good (TRG 1–2) 18명 / Mid (TRG 3) 19명 / Non (TRG 3-4 이상) 14명 |

## 원 논문 분석과 다른 점
- **전체 유전자 사용**: GEO에 올라온 xlsx 행렬은 검출률 낮은 유전자를 걸러 10,003개만 있음 → 원본 카운트(DCC) + 프로브 표(PKC)로 18,676개 유전자 행렬을 다시 만들고 Q3 정규화
- **중간 반응군 포함**: Good vs Non 두 군 비교 대신 TRG 순서에 따른 단계적 경향(스피어만)으로 51명 전체 사용
- **환자 단위 통계**: 같은 환자의 여러 코어는 평균
- **교란 보정**: 슬라이드 런(4개)과 segment 핵 수를 넣은 선형회귀
- **주요 지표 사전 지정**: 기질 IgG ↔ TRG

## 주요 결과

| 지표 | TRG ρ | p | 보정 p |
|---|---|---|---|
| 기질 IgG (IGHG1–4) — 주요 지표 | 0.34 | 0.023 | 0.023 |
| 기질 IGKC | 0.37 | 0.013 | 0.006 |
| 기질 IgA (IGHA1) | 0.31 | 0.042 | 0.033 |
| 종양 구획 IGKC | 0.40 | 0.006 | 0.010 |
| CD8 종양 − 기질 (탐색적) | −0.24 | 0.125 | — |

- 면역글로불린 신호가 Good → Mid → Non 순으로 단계적으로 증가하고, 보정 후에도 유지됨
- IgA도 같은 방향 → IgG만의 변화라기보다 형질세포/IG 전반의 증가
- 모든 반응군에서 IG는 종양 구획보다 기질 구획에 훨씬 많음 (반응군별 Wilcoxon p < 0.001)
- CD8 신호는 대부분 segment에서 배경 수준이라 판단 어려움

## 한계
- 구획 단위 데이터라 형질세포 수 증가인지 세포당 발현 증가인지 구분 불가, 세포 수준 공간 배치(경계로부터 거리 등)는 볼 수 없음
- 원 논문에 환자마다 nCRT 요법이 다를 수 있다고 명시됨
- 정상 점막 혼입(IgA 형질세포가 많음)에 의한 영향은 아직 확인하지 않음
- 단일 데이터셋, 51명. 다른 코호트에서의 재현 필요

## 실행 방법
1. `GSE279942_IG_response.ipynb`를 Google Colab에서 열기
2. 1번 셀의 `BASE`를 원하는 드라이브 경로로 수정
3. 위에서부터 차례로 실행 — GEO에서 원본 파일(약 10 MB)을 자동으로 받음
4. 결과는 `BASE/final/`에 저장

## 재현 정보
- `results/run_info.json`: 실행 시각, 파이썬·패키지 버전, 시드, GEO 입력 파일 SHA-256, 결과 표 SHA-256, 핵심 수치
- `results/requirements_frozen.txt`: 실행 환경 전체 패키지 목록
- 주요 버전: Python 3.13, numpy 2.4.6, pandas 2.2.3, scipy 1.16.3, statsmodels 0.15.0, matplotlib 3.10.0
- 다시 실행했을 때 `patient_scores_sha256`이 같으면 결과가 완전히 동일하게 재현된 것

## 구성
```
├── README.md
├── GSE279942_IG_response.ipynb     # 전체 분석 (데이터 받기 → 정규화 → 통계 → 그림)
└── results/
    ├── Fig_GSE279942_IG_by_response.png
    ├── patient_scores.csv            # 환자 × 점수
    ├── stats_trend_pairwise.csv      # TRG 경향, 보정 p, 두 군씩 비교
    ├── stats_IgG_tumor_vs_stroma.csv # 같은 환자 종양 vs 기질
    ├── run_info.json
    └── requirements_frozen.txt
```

## 참고
- 원본 데이터의 권리는 원 저자에게 있으며, 이 저장소는 GEO 원본 파일을 포함하지 않음 (노트북이 GEO에서 직접 받음)
- 코드 작성에 AI(Claude)를 활용했고, 분석 설계와 결과 검증은 직접 수행함
