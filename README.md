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

**Web / Backend**

![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)

**Infra / Deploy**

![AWS Elastic Beanstalk](https://img.shields.io/badge/AWS_Elastic_Beanstalk-232F3E?style=flat-square&logo=amazonaws&logoColor=white)
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
└─────────────────────────────┬───────────────────┘
                              │
          ┌───────────────────┼───────────────────┐
          ▼                                       ▼
  [부정적 감정 예측]                        [안정적 감정 예측]
          │                                       │
          ▼                                       ▼
  외부 데이터 기반                          격려 메시지만
  솔루션 후보 추출                            JSON 출력
          │
          ▼
  RAG (감정·행동 논문 기반 ChromaDB)
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

---

## ✨ Main Features

### 🌐 Django 웹 서비스 및 반응형 UI
- Django 기반 전체 웹 서비스 구현
- JavaScript를 활용한 모바일(스마트폰) 반응형 UI 구성

**(ENG)** Built the full web service with Django and implemented a mobile-responsive UI using JavaScript.

---

### 🚀 AWS 자동 배포 (CI/CD)
- AWS Elastic Beanstalk 기반 배포 환경 구성
- GitHub Actions 연동으로 push 시 자동 배포 파이프라인 구축

**(ENG)** Configured deployment on AWS Elastic Beanstalk and built an automated CI/CD pipeline triggered by GitHub Actions on push.

---

### 🧠 감정 예측 (LSTM)
- 하루 3회 기록된 감정 데이터를 시계열로 학습
- LSTM 모델로 다음 날의 감정 상태를 예측
- 예측 결과를 기반으로 분기 처리 (부정 / 안정)

**(ENG)** Time-series emotion data (3 records/day) is fed into an LSTM model to predict the next day's emotional state, which then determines the recommendation flow.

---

### 💡 감정 완화 솔루션 추천 (RAG)
- 감정 및 행동 관련 논문 수집 → ChromaDB 구축
- 외부 데이터로 후보 솔루션 추출
- RAG 파이프라인을 통해 감정 완화 행동 솔루션 + 응원 메시지를 JSON으로 출력

**(ENG)** Research papers on emotion and behavior are embedded into ChromaDB. Candidate solutions are extracted from external data, then passed through a RAG pipeline to generate behavior recommendations and motivational messages as JSON output.

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
- ChromaDB를 활용한 RAG 파이프라인 구축
- 논문 기반 외부 지식을 LLM 추천 시스템에 통합하는 방법
- 통계 기반과 RAG 기반 추천 방식의 차이 비교
- FastAPI + Docker 기반 AI 서비스 배포 경험

**(ENG)**
- Designing LSTM-based time-series emotion prediction pipelines
- Building RAG pipelines using ChromaDB
- Integrating research paper knowledge into LLM-based recommendation systems
- Comparing statistical vs. RAG-based recommendation approaches
- Deploying AI services using FastAPI and Docker



# AI Life & Food Advisor
<img width="1920" height="919" alt="image" src="Projects/static/readme/PPT_첫 페이지.jpg">

## 📌 프로젝트 계획서

### 1. 프로젝트 개요

* **프로젝트명**: AI Life & Food Advisor
* **목표**: 감정·식단 데이터를 통합하여 수집한 뒤, 사용자에 대한 일일/주간 리포트와 행동 추천 및 메뉴 추천을 제공
* **기간**: 2025년 11월 \~ 2026년 1월

### 2. 프로젝트 일정

* **기획 및 설계**: 11월 중순
* **데이터 모델링**: 11월 말 \~ 12월 초
* **모델 개발(ML/LLM)**: 12월 초 \~ 12월 중순
* **UI/UX 구현 및 테스트/배포**: 12월 중순 \~ 1월 초
* **최종 발표 및 보고서 작성**: 1월 초 \~ 1월 9일

---

## 😎 팀원 소개
<table style="width:100%; text-align:center; table-layout:fixed; border-collapse:collapse;">
  <colgroup>
    <!-- 전체 5열이므로 20%씩 -->
    <col style="width:20%;">
    <col style="width:20%;">
    <col style="width:20%;">
    <col style="width:20%;">
  </colgroup>
  <thead>
    <tr>
      <th>김민정</th>
      <th>김남기</th>
      <th>유 정</th>
      <th>장형준</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><img src="Projects/static/readme/Profile_Img_김민정.jpg" style="width:150px; height:auto;"></td>
      <td><img src="Projects/static/readme/Profile_Img_김남기.jpg" style="width:150px; height:auto;"></td>
      <td><img src="Projects/static/readme/Profile_Img_유정.jpg" style="width:150px; height:auto;"></td>
      <td><img src="Projects/static/readme/Profile_Img_장형준.jpg" style="width:150px; height:auto;"></td>
    </tr>
    <tr>
      <td align="center" valign="middle">PM<br>ML/LLM<br>Front/Back</td>
      <td align="center" valign="middle">ML<br>Front/Back<br>AWS</td>
      <td align="center" valign="middle">ML<br>Front/Back<br>AWS</td>
      <td align="center" valign="middle">ML<br>Front/Back<br>RDS</td>
    </tr>
    <!-- <tr>
      <td><a href="https://github.com/qkrcool" target="_blank">
          <img src="https://img.shields.io/badge/GitHub-Link-black?style=flat&logo=github&logoColor=white" />
        </a></td>
      <td><a href="https://github.com/strangem1n" target="_blank">
          <img src="https://img.shields.io/badge/GitHub-Link-black?style=flat&logo=github&logoColor=white" />
        </a></td>
      <td><a href="https://github.com/jisunclaralee" target="_blank">
          <img src="https://img.shields.io/badge/GitHub-Link-black?style=flat&logo=github&logoColor=white" />
        </a></td>
      <td><a href="https://github.com/saeyeonIm" target="_blank">
          <img src="https://img.shields.io/badge/GitHub-Link-black?style=flat&logo=github&logoColor=white" />
        </a></td>
      <td><a href="https://github.com/tjdud1199" target="_blank">
          <img src="https://img.shields.io/badge/GitHub-Link-black?style=flat&logo=github&logoColor=white" />
        </a></td>
    </tr> -->
  </tbody>
</table>

---

## 🛠️ 기술 스택
### - Languages
<img src="https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white" /> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black" />
<img src="https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white" /> <img src="https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white" />
### - Framework
<img src="https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white" />

### - ML/LLM
<img src="https://img.shields.io/badge/LangChain-1C3C3C?style=flat" /> <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white" /> <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white" /> <img src="https://img.shields.io/badge/pandas-150458?style=flat&logo=pandas&logoColor=white" />
### - Infrastructure
<img src="https://img.shields.io/badge/AWS-232F3E?style=flat&logo=Amazon%20AWS&logoColor=white" /> (  <img src="https://img.shields.io/badge/EC2-FF9900?style=flat&logo=Amazon%20EC2&logoColor=white" />   <img src="https://img.shields.io/badge/S3-569A31?style=flat&logo=Amazon%20S3&logoColor=white" />  <img src="https://img.shields.io/badge/CloudFront-A374DB?style=flat&logo=Amazon%20AWS&logoColor=white" />  <img src="https://img.shields.io/badge/Elastic%20Beanstalk-FF9900?style=flat&logo=Amazon%20AWS&logoColor=white" />)
### - Database
<img src="https://img.shields.io/badge/Amazon%20RDS-527FFF?style=flat&logo=Amazon%20RDS&logoColor=white" /> <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white" />
### - API
<img src="https://img.shields.io/badge/FatSecret%20API-4CAF50?style=flat" /> <img src="https://img.shields.io/badge/OpenAI%20API-412991?style=flat&logo=openai&logoColor=white" /> <img src="https://img.shields.io/badge/식품의약품안전처%20API-005BAC?style=flat" /> <img src="https://img.shields.io/badge/공공데이터포털-003964?style=flat" />
### - Collaboration
<img src="https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white" /> <img src="https://img.shields.io/badge/Notion-000000?style=flat&logo=notion&logoColor=white" /> <img src="https://img.shields.io/badge/SourceTree-0052CC?style=flat&logo=sourcetree&logoColor=white" /> <img src="https://img.shields.io/badge/Canva-00C4CC?style=flat&logo=canva&logoColor=white" />

---
## 📄 최종 보고서
- [최종 보고서 (발표 PDF)](./쨔잔_최종보고서.pdf)
