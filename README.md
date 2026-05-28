# 🐻 BEAR — Behavior & Emotion AI Recommender

> 감정 예측 기반 행동 솔루션 및 맞춤 식단 추천 AI 시스템  
> **(ENG) Emotion-Prediction-Based Behavior Solution & Personalized Diet Recommendation AI System**

---

## 📌 Project Summary

사용자가 **하루 세 번** 감정 및 식단 데이터를 기록하면,  
LSTM 기반 감정 예측 모델이 **다음 날의 감정 상태를 예측**합니다.

전체 웹 서비스는 **Django** 기반으로 구현되었으며, JavaScript를 활용해 **모바일(스마트폰) 반응형**으로 제작되었습니다.  
배포는 **AWS Elastic Beanstalk + GitHub Actions 연동을 통한 자동 배포(CI/CD)** 로 구성했습니다.

예측 결과에 따라:
- **부정적 감정 예측 시** → RAG 기반 감정 완화 행동 솔루션 + 응원 메시지 추천
- **안정적 감정 예측 시** → 솔루션 없이 격려 메시지만 제공

식단 데이터는 사용자가 설정한 **탄·단·지 비율**에 맞춰  
통계 기반 방식과 RAG 기반 방식 **두 가지 방법**으로 하루 식단을 추천합니다.

---

**(ENG)**

Users record **emotion and diet data three times a day**.  
An LSTM-based model then **predicts the user's emotional state for the next day**.

The entire web service is built with **Django** and made **mobile-responsive using JavaScript**.  
Deployment is fully automated via **AWS Elastic Beanstalk integrated with GitHub Actions (CI/CD)**.

Based on the prediction:
- **Negative emotion predicted** → RAG-based behavior solution recommendations + motivational message
- **Stable emotion predicted** → Encouraging message only, no solution recommended

For diet, the system recommends a full day's meal plan that matches the user's configured **carb/protein/fat ratio**, using **two methods**: a statistical approach and a RAG-based approach.

---

## 🛠 Skills
**AI / ML**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![LSTM](https://img.shields.io/badge/LSTM-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![RAG](https://img.shields.io/badge/RAG-6DB33F?style=flat-square&logoColor=white)
![ChromaDB](https://img.shields.io/badge/ChromaDB-FF4785?style=flat-square&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3C?style=flat-square&logo=langchain&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-150458?style=flat&logo=pandas&logoColor=white)

**Web / Backend**

![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)

**Infra / Deploy**

![AWS Elastic Beanstalk](https://img.shields.io/badge/AWS_Elastic_Beanstalk-232F3E?style=flat-square&logo=amazonaws&logoColor=white)
![AWS RDS](https://img.shields.io/badge/Amazon%20RDS-527FFF?style=flat&logo=Amazon%20RDS&logoColor=white)
![AWS EC2](https://img.shields.io/badge/EC2-FF9900?style=flat&logo=Amazon%20EC2&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

---

## 🔄 System Architecture

```
사용자 입력 (감정 + 식단 데이터 × 하루 3회)
        │
        ▼
┌─────────────────────────────────────────────────┐
│              감정 예측 모듈 (LSTM)                │
│  과거 감정 기록 → 다음날 감정 상태 예측            │
│  (1M+ records · 7 features · recall ≥ 70%)      │
└─────────────────────────────┬───────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                                       ▼
  [부정적 감정 예측]                        [안정적 감정 예측]
          │                                       │
          ▼                                       ▼
  행동 후보 스코어링                          격려 메시지만
  (Rule-based + CF/SVD 하이브리드)             JSON 출력
          │
          ▼
  RAG (감정·행동 논문 15편 기반 ChromaDB)
          │
          ▼
  행동 솔루션 + 응원 메시지 → JSON 출력 → 화면 표시

─────────────────────────────────────────────────

사용자 탄·단·지 비율 설정
        │
        ├─── ① 통계 기반 식단 추천
        │
        └─── ② RAG 기반 식단 추천
                (식단·감정 관련 논문 기반 ChromaDB)
```

**(ENG)**

```
User Input (emotion + meal data × 3 times/day)
        │
        ▼
┌─────────────────────────────────────────────────┐
│           Emotion Prediction Module (LSTM)       │
│  Past emotion records → Next-day state prediction│
│  (1M+ records · 7 features · recall ≥ 70%)      │
└─────────────────────────────┬───────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                                       ▼
  [Negative Emotion Predicted]        [Stable Emotion Predicted]
          │                                       │
          ▼                                       ▼
  Behavior Candidate Scoring                Encouraging message
  (Rule-based + CF/SVD Hybrid)               JSON output only
          │
          ▼
  RAG (ChromaDB · 15 emotion-behavior papers)
          │
          ▼
  Behavior solution + motivational message → JSON → UI

─────────────────────────────────────────────────

User sets carb / protein / fat ratio
        │
        ├─── ① Statistical-based meal recommendation
        │
        └─── ② RAG-based meal recommendation
                (ChromaDB built from diet-emotion research papers)
```

---

## ✨ Main Features

### 🧠 감정 예측 (LSTM)
- 1,000명 사용자의 **1,000,940행** 행동·감정 데이터를 전처리 (Kaggle)
- **7개 피처**를 선별해 LSTM 시계열 모델 구축, **재현율(Recall) ≥ 70%** 목표로 학습
- 예측 결과를 기반으로 분기 처리 (부정 / 안정)

**(ENG)**  
Preprocessed 1,000,940 behavioral records from 1,000 Kaggle users down to 7 features, then trained an LSTM time-series model targeting recall ≥ 70% for next-day emotional risk prediction.

---

### 💡 행동 솔루션 추천 — 하이브리드 스코어링 (Rule-based + CF/SVD)

행동 추천은 단순 RAG가 아닌 **두 단계 스코어링 파이프라인**으로 구성됩니다.

**Step 1. Rule-based Score**

감정 valence(기분)와 arousal(각성)의 변화량을 기반으로 행동 효과 점수를 산출합니다.

```python
# arousal은 변화가 적을수록 좋음 (너무 들뜨거나 가라앉는 것 모두 부담)
df['arousal_effect'] = -(df['arousal_delta_norm'].abs())

# valence 개선이 더 중요하므로 가중합으로 종합 점수 산출
df['score'] = 0.6 * df['valence_delta_norm'] + 0.4 * df['arousal_effect']
```

기분이 실제로 개선된 행동만 필터링(`valence_delta > 0`)한 뒤,  
행동 빈도(신뢰성)를 30% 반영한 **final_score**로 최종 랭킹을 산출합니다.

```python
pos_effect['final_score'] = 0.7 * pos_effect['score'] + 0.3 * pos_effect['count_norm']
```

| Activity | Final Score |
|---|---|
| gym workout | 0.605 |
| swimming | 0.604 |
| walking | 0.604 |
| cycling | 0.603 |
| running | 0.603 |

**Step 2. CF/SVD Score**

Rule-based 결과에 협업 필터링(SVD)을 결합해 사용자 패턴 기반 예측 점수를 산출하고,  
두 점수를 **50:50 가중합**하여 최종 추천 행동 Top 3를 선정합니다.

```python
df['final_score'] = 0.5 * df['rule_score'] + 0.5 * df['cf_score']
```

**Step 3. RAG**

선정된 Top 3 행동과 감정·행동 관련 **논문 15편**을 ChromaDB에 인덱싱한 벡터 DB를 결합해,  
LLM이 맥락에 맞는 행동 솔루션과 응원 메시지를 생성합니다.

**(ENG)**  
Behavior recommendations follow a three-stage pipeline: (1) rule-based valence/arousal scoring to rank activities by emotional effect, (2) CF+SVD collaborative filtering combined at 50:50 weight to personalize rankings, and (3) RAG over 15 research papers indexed in ChromaDB to generate context-aware LLM messages.

---

### 🚀 AWS 자동 배포 (CI/CD) — 배포 시간 54분 → 1분 30초

- AWS Elastic Beanstalk 기반 배포 환경 구성
- GitHub Actions 연동으로 push 시 자동 배포 파이프라인 구축
- 초기 배포 시 **타임아웃(54분)** 으로 배포 실패 발생
- ML 패키지(TensorFlow 등) 를 별도 레이어로 분리해 **평균 배포 시간 1분 30초**로 단축

**(ENG)**  
Configured AWS Elastic Beanstalk with a GitHub Actions CI/CD pipeline. Resolved a critical deployment timeout (54 min) caused by heavy ML package installation by separating packages into isolated layers, reducing average deploy time to ~1.5 min.

---

### 🌐 Django 웹 서비스 및 반응형 UI
- Django 기반 전체 웹 서비스 구현
- JavaScript를 활용한 모바일(스마트폰) 반응형 UI 구성

**(ENG)** Built the full web service with Django and implemented a mobile-responsive UI using JavaScript.

---

### 🥗 맞춤 식단 추천 (2가지 방식)
- 사용자가 탄·단·지 비율을 직접 설정
- **방식 1**: 통계 기반 식단 구성 (영양소 계산)
- **방식 2**: 식단·감정 관련 논문 기반 ChromaDB + RAG를 활용한 식단 추천

**(ENG)** Users set their preferred carb/protein/fat ratio. The system recommends a full-day meal plan using two approaches: a statistical nutrient calculation method and a RAG-based method using a ChromaDB built from diet-emotion research papers.

---

### 📤 JSON 출력 구조

```json
{
  "emotion_prediction": "negative",
  "solutions": [
    {
      "action": "산책 30분",
      "reason": "가벼운 유산소 운동이 세로토닌 분비를 촉진합니다."
    }
  ],
  "message": "오늘 하루도 정말 잘 버텼어요. 내일은 더 나아질 거예요 💪"
}
```

---

## 💡 Why We Built This

감정 데이터와 식단 데이터를 함께 다루며,  
단순 기록을 넘어 **예측 → 솔루션 추천**까지 이어지는  
실질적인 AI 파이프라인을 경험하고자 했습니다.

LSTM 기반 시계열 예측과 RAG 기반 추천을 결합해  
**머신러닝과 LLM 기술을 실제 서비스 흐름에 적용**하는 것이 핵심 목표였습니다.

**(ENG)**  
We wanted to build an end-to-end AI pipeline that goes beyond simple logging —  
from **prediction to solution recommendation** — using both emotion and diet data.  
The core goal was to integrate LSTM-based time-series prediction with RAG-based LLM recommendations in a real service flow.

---

## 📂 Project Structure

```text
BEAR/
├── emotion/
│   ├── lstm_model/          # LSTM 감정 예측 모델
│   └── rag_pipeline/        # 감정 완화 솔루션 RAG
├── diet/
│   ├── statistical/         # 통계 기반 식단 추천
│   └── rag_pipeline/        # RAG 기반 식단 추천
├── chromadb/
│   ├── emotion_papers/      # 감정·행동 논문 벡터 DB
│   └── diet_papers/         # 식단·감정 논문 벡터 DB
├── api/                     # FastAPI 서버
├── docker-compose.yml
└── requirements.txt
```

---

## 🧑‍💻 What I Learned

- LSTM을 활용한 시계열 감정 데이터 예측 구조 설계
- Rule-based + CF/SVD 하이브리드 스코어링 파이프라인 설계
- ChromaDB를 활용한 RAG 파이프라인 구축
- 논문 기반 외부 지식을 LLM 추천 시스템에 통합하는 방법
- 통계 기반과 RAG 기반 추천 방식의 차이 비교
- FastAPI + Docker 기반 AI 서비스 배포 경험
- AWS 배포 타임아웃 문제 진단 및 패키지 분리를 통한 성능 개선

**(ENG)**
- Designing LSTM-based time-series emotion prediction pipelines
- Building a hybrid Rule-based + CF/SVD scoring pipeline for behavior ranking
- Building RAG pipelines using ChromaDB
- Integrating research paper knowledge into LLM-based recommendation systems
- Comparing statistical vs. RAG-based recommendation approaches
- Deploying AI services using FastAPI and Docker
- Diagnosing and resolving AWS deployment timeout via package layer separation
