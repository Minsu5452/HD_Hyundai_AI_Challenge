# 선박 대기시간 예측

HD현대 AI Challenge에서 선박 입항 후 대기시간을 예측한 예선 솔루션과, 본선 시계열 회귀 실험을 함께 정리한 프로젝트입니다.

## 개요

| 항목 | 내용 |
| --- | --- |
| 대회 | HD현대 AI Challenge |
| 기간 | 2023.09.25 - 2023.11.10 |
| 주최 / 주관 | HD한국조선해양 AI Center / DACON |
| 팀 구성 | 3인팀, 팀장 |
| 결과 | 예선 2등 / 330팀, 본선 6등 / 11팀 |
| 주요 과제 | 선박 대기시간 `CI_HOUR` 회귀 예측 |
| 본선 과제 | 시계열 신호 기반 trial weight 회귀 |

## 접근

- 예선에서는 결측 처리, 시간 파생 변수, target encoding, XGBoost/CatBoost ensemble을 구성했습니다.
- `DIST == 0` 등 특수 케이스를 후처리해 회귀 예측을 보정했습니다.
- 본선에서는 시계열 신호 파일을 trial 단위로 파싱하고 sliding window feature를 구성했습니다.
- LightGBM 예측을 ID 단위 percentile 집계와 rounding으로 제출 형식에 맞췄습니다.

## 저장소 구성

```text
.
├── notebooks/
│   ├── preliminaries/
│   │   ├── 01_eda_and_feature_engineering.ipynb
│   │   └── 02_xgboost_catboost_ensemble.ipynb
│   └── finals/
│       ├── 01_signal_eda_and_feature_engineering.ipynb
│       └── 02_lightgbm_pipeline.ipynb
└── requirements.txt
```

## 공개 범위

대회 원본 데이터, 전처리 산출물, 모델 파일, 제출 파일은 포함하지 않았습니다. 노트북 출력과 로컬 메타데이터를 정리했습니다.

## 링크

- [DACON 대회 페이지](https://dacon.io/competitions/official/236158/overview/description)
