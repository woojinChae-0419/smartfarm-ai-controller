# 🌱 스마트팜 AI 제어 시스템

> 군산대학교 임베디드소프트웨어학과 졸업작품  
> 멀티모달 AI(이미지 + 센서 데이터)를 활용한 식물 상태 자동 진단 및 제어 시스템

---

## 📌 프로젝트 개요

| 항목 | 내용 |
|------|------|
| 학기 | 1학기: 소프트웨어/모델 구현 |
| 목표 | 식물 이미지 + 센서 데이터를 멀티모달 AI로 융합해 식물 상태 자동 분류 |
| 분류 클래스 | 정상(Healthy) / 병해(Disease) / 수분부족(Drought) / 성장단계(Growth) |
| 2학기 계획 | 라즈베리파이 + 실제 센서 연동 + 자동 제어(펌프/LED/팬) |

---

## 🗂️ 1학기 블럭도

👉 **[블럭도 전체 보기](./smartfarm_blockdiagram.html)**  
*(GitHub Pages 또는 브라우저에서 직접 열어서 확인)*

### 전체 흐름

```
[데이터 수집]
  식물 이미지 데이터셋 (PlantVillage ~54,000장)
  + 센서 시뮬레이션 데이터 (온습도/토양수분/조도/CO2)
  + 라벨 정의 (4클래스)
        ↓
[전처리]
  이미지: Resize 224×224, Normalize, Augmentation
  센서: Min-Max 정규화, IQR 이상값 제거
  데이터셋: Train 70 / Val 15 / Test 15 (Stratified split)
        ↓
[AI 모델 학습 — 멀티모달 융합]
  이미지 인코더: EfficientNet-B0 파인튜닝 (→ 1280-dim feature)
  센서 인코더:   MLP 3층 (→ 64-dim feature)
  융합 레이어:   Concat(1344) → FC → Softmax → 4클래스
        ↓
[추론 결과]
  클래스 예측 + 신뢰도(confidence) 출력
        ↓
[출력 / 서비스]
  Streamlit 대시보드  |  Claude API 진단 리포트  |  모델 성능 리포트
```

---

## 🛠️ 기술 스택

| 분류 | 라이브러리 / 도구 |
|------|-----------------|
| AI 프레임워크 | PyTorch, torchvision |
| 이미지 모델 | EfficientNet-B0 (pretrained) |
| 데이터 전처리 | pandas, scikit-learn |
| 대시보드 | Streamlit, Plotly |
| AI 리포트 | Claude API (claude-sonnet-4-5) |
| 시각화 | matplotlib, seaborn |
| 실험 관리 | TensorBoard |

---

## 📁 프로젝트 구조 (예정)

```
smartfarm-ai/
├── data/
│   ├── raw/                  # 원본 이미지 데이터셋
│   ├── simulated_sensor/     # 센서 시뮬레이션 데이터 (CSV)
│   └── processed/            # 전처리 완료 데이터
├── models/
│   ├── image_encoder.py      # EfficientNet-B0 파인튜닝
│   ├── sensor_encoder.py     # MLP 센서 인코더
│   └── fusion_model.py       # 멀티모달 융합 모델
├── train/
│   ├── train.py              # 학습 메인 스크립트
│   ├── evaluate.py           # 평가 및 성능 리포트
│   └── config.yaml           # 하이퍼파라미터 설정
├── dashboard/
│   └── app.py                # Streamlit 대시보드
├── report/
│   └── claude_report.py      # Claude API 진단 리포트 생성
├── smartfarm_blockdiagram.html  # 1학기 블럭도
└── README.md
```

---

## 🚀 실행 방법

```bash
# 의존성 설치
pip install -r requirements.txt

# 센서 시뮬레이션 데이터 생성
python data/generate_sensor_data.py

# 모델 학습
python train/train.py --config train/config.yaml

# Streamlit 대시보드 실행
streamlit run dashboard/app.py
```



## 👤 개발자

- 군산대학교 임베디드소프트웨어학과

