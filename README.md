# 🚢 HD현대 AI Challenge — 선박 대기시간 예측

![Preliminary](https://img.shields.io/badge/예선%202nd-330%20teams-silver)
![Final](https://img.shields.io/badge/본선%206th-11%20teams-blue)
![Host](https://img.shields.io/badge/Host-HD한국조선해양%20AI%20Center-navy)
![Venue](https://img.shields.io/badge/Final-현장%20발표%20(오프라인)-red)

> **예선 2nd / 330팀 (상위 0.6%) · 본선 6th / 11팀 (현장 발표)** — HD현대(HD한국조선해양) 주최 공식 AI Challenge. 예선 온라인 + **본선 현장 오프라인 발표**. Boosting 계열 앙상블 + Transformer 기반 시계열 예측.

---

## 🏆 Awards

- 🥈 **예선 2위** · HD현대 AI Challenge · 330팀 · 2023.09.25 ~ 2023.10.30
- 🎤 **본선 6위** · 11팀 · 현장 오프라인 발표
- 주최: HD한국조선해양 AI Center / 운영: DACON

## 🔍 Overview

- **배경**: 해운·조선 물류에서 선박 항구 대기시간 예측은 연료·스케줄·부두 운영 최적화의 핵심.
- **문제 정의**: 선박 운항·항구·날씨·화물 등 tabular + 시계열 데이터로 **대기시간(regression) 예측**.
- **난점**:
  1. **다양한 데이터 소스** — 정적(선박 제원) + 동적(항구 상태) + 시계열(날씨) 결합
  2. **이상치 다수** — 장기 체류 선박의 꼬리 분포
  3. **도메인 특화 특성** — 항로·선박 타입별 이질성

## 🧠 Approach

### 예선 (팀장 · 3인 팀)
- **Boosting 앙상블** (XGBoost · LightGBM · CatBoost)
- 도메인 피처 엔지니어링 (항구별 집계 · 선박 타입 범주화)
- K-Fold CV + Target Encoding
- **결과: 예선 2nd / 330팀**

### 본선 (팀원 · 3인 팀)
- **Transformer 기반 시계열 모델** 실험
- 예선 앙상블 모델과 하이브리드
- 현장 발표 준비 (슬라이드·모델 해석)
- **결과: 본선 6위 / 11팀**

### 파이프라인
```
Raw data (ship · port · weather · cargo)
    │
    ▼  Feature Engineering
- 항구별 시간대 집계
- 선박 타입/항로 범주화
- 날씨 시계열 윈도우
    │
    ▼  Boosting Ensemble (예선)
XGBoost · LightGBM · CatBoost (K-Fold)
    │
    ▼  Transformer (본선)
Time-aware attention
    │
    ▼  Stacking / Blending
최종 예측
```

## 📈 Results

| 단계 | 순위 | 규모 | 비고 |
|------|------|------|------|
| **예선** | 🥈 **2위** | 330팀 | 상위 0.6%, 팀장 3인 |
| **본선** | 6위 | 11팀 | 수상권(5위)까지 1위 차이, 현장 발표 |

## 🛠 Tech Stack

- **Language**: Python 3.8+
- **ML**: XGBoost · LightGBM · CatBoost · scikit-learn
- **DL**: PyTorch (Transformer)
- **Data**: Pandas · NumPy
- **Env**: Jupyter Notebook

## 📁 Structure

```
HD_Hyundai_AI_Challenge/
├── HD_Preliminaries/     # 예선 솔루션 (Boosting 앙상블)
├── HD_Finals/            # 본선 솔루션 (Transformer + Hybrid)
└── README.md
```

## 👤 Role / Team

- **역할**: 예선 팀장 → 본선 팀원 (3인 팀)
- **기여**:
  - 예선: 피처 엔지니어링 주도 · 앙상블 전략 수립 · 팀 리드
  - 본선: Transformer 모델 실험 · 현장 발표 지원

## 🔗 Links

- [DACON 대회 페이지](https://dacon.io/competitions/official/236158/overview/description)

---

> 🔗 Portfolio: [Minsu5452](https://github.com/Minsu5452)
