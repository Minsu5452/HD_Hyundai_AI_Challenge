# Ship Waiting Time Prediction

> 2023 HD현대 AI Challenge 참여 프로젝트입니다. 예선 단계의 **선박 입항 후 대기시간(`CI_HOUR`) 회귀 예측** 을 메인 과제로, 본선 단계의 시계열 신호 → trial 단위 weight 회귀를 sub-task 로 함께 정리했습니다.

## Overview

| 항목 | 내용 |
| --- | --- |
| 대회 | HD현대 AI Challenge |
| 예선 | 2023.09.25 ~ 2023.10.30 |
| 본선 (온라인 모델링) | 2023.11.06 ~ 2023.11.09 |
| 본선 (오프라인 발표평가) | 2023.11.10 |
| 주최 | HD한국조선해양 AI Center |
| 주관 | DACON |
| 팀 구성 | 3인팀, 팀장 |
| 결과 | 예선 2위 / 330팀, 본선 6위 / 11팀 |
| 메인 평가지표 | MAE |

## Stages

본 대회는 예선과 본선이 **별개의 데이터셋과 과제** 로 구성되어 있어, 노트북도 단계별로 분리했습니다.

| 단계 | 입력 | 타겟 | 모델 |
| --- | --- | --- | --- |
| 예선 | 정형 표 (`train.csv`, `test.csv`) | 선박 대기시간 `CI_HOUR` (회귀) | XGBoost + CatBoost 앙상블 |
| 본선 | 시계열 신호 CSV 다수 (`train/`, `test/`) | trial 단위 `weight` (회귀) | LightGBM + ID 단위 percentile 집계 |

## Approach

### 예선

- `BREADTH` 결측 행 제거, 기상 변수의 위장 결측을 NaN 으로 통일했습니다.
- `ATA` 에서 시간 파생 피처 생성, 학습 셋에서만 집계한 `weather_mean` 파생 피처를 양 셋에 매핑(누설 방지).
- `LabelEncoder` + 세 가지 target encoding(`CO_PO_mean`, `CO_PO_ym_mean`, `CO_PO_SH_y_mean`).
- `weather_mean` 결측 여부로 데이터를 분기, CatBoost 파이프라인은 `DIST > 0` 추가 분기 + `DIST == 0` 후처리로 0 보정.
- 두 파이프라인 각각 5-fold KFold + early stopping → 0.5/0.5 블렌딩 후 음수 클리핑.

### 본선

- 파일명에서 trial 라벨/드라이버를 파싱하고 신호 컬럼을 train+test 전역 min/max 로 정규화했습니다.
- 각 파일을 500 step 슬라이딩 윈도우로 슬라이스하고 파일 단위로 8:2 train/valid split.
- LightGBM + early stopping(200) 으로 단일 모델 학습, 윈도우 단위 예측을 ID 단위 24.5 percentile 로 집계 후 `round(-2)` 후처리.

## Repository Structure

```text
.
├── notebooks/
│   ├── preliminaries/
│   │   ├── 01_eda_and_feature_engineering.ipynb
│   │   └── 02_xgboost_catboost_ensemble.ipynb
│   └── finals/
│       ├── 01_signal_eda_and_feature_engineering.ipynb
│       └── 02_lightgbm_pipeline.ipynb
├── requirements.txt
└── README.md
```

## Public Scope

이 저장소는 포트폴리오 공개용으로 정리한 버전입니다.

- 대회 제공 원본 데이터, 전처리 산출물(`data/processed/`), 학습된 모델 파일, 제출 CSV 는 포함하지 않았습니다.
- 노트북 출력 결과와 실행 메타데이터는 제거했습니다.
- 이전 버전의 단일 노트북(예선 1개 + 본선 2개)은 데이터 로드/전처리/스플릿이 한 셀에 길게 섞여 있어, 단계별 4개 노트북으로 재구성했습니다.
- 본선 Transformer 실험 노트북은 import 만 있는 빈 더미라 폐기했습니다.
- 실행하려면 DACON 대회 데이터를 `data/` 경로에 별도로 배치해야 합니다.

## Links

- [DACON competition page](https://dacon.io/competitions/official/236158/overview/description)
