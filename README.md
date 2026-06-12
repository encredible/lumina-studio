# 🌌 Lumina Studio (루미나 스튜디오)

> **Lumina Studio**는 도커(Docker) 기반의 프리미엄 로컬/원격 인공지능 개발 워크스페이스입니다. 모델 다운로드, 데이터셋 가공, 합성 데이터 생성, LoRA 파인튜닝, 멀티 API 플레이그라운드 비교 분석 환경을 단 하나의 컨테이너 안에서 모두 제공합니다.

---

## 🌟 주요 기능

### 1. 🐳 컨테이너화 및 이식성
- 가볍고 정교한 `Dockerfile` 및 `docker-compose.yml`을 지원하여 한 줄의 명령어로 즉시 구동 가능합니다.
- 크로스 플랫폼을 지원하는 시스템 텔레메트리(CPU 사용량, RAM 사용률, 디스크 용량, Python 및 PyTorch 가속기 환경 조회)가 대시보드에 실시간 표시됩니다.
- 저비용/고호환 환경을 위해 기본적으로 CPU 기반 PyTorch로 빌드되며, GPU 및 Apple Silicon(MPS) 가속 역시 원활히 지원합니다.

### 2. 📥 멀티 사이트 모델 다운로더
- **Hugging Face Hub**: 전체 레포지토리 스냅샷 다운로드 또는 개별 가중치/설정 파일만 쏙 골라 다운로드 가능.
- **Alibaba ModelScope**: 공식 SDK(`snapshot_download`)를 내장하여 빠르고 유연하게 모델 다운로드 지원.
- **Civitai**: 리디렉션 감지 로직을 내장하여 Stable Diffusion 등 이미지 모델 다운로드 완벽 지원.
- **Direct HTTP/HTTPS URL**: 파일 분할 스트리밍 다운로드 기능으로 진행률, 다운로드 속도 및 용량을 실시간 표시.
- **원클릭 모델 프리셋**: Qwen-2.5-0.5B, TinyLlama-1.1B, OPT-125m 등 가벼운 모델을 클릭 한 번에 양식 기입 및 다운로드 준비 완료.

### 3. 📊 인터랙티브 데이터셋 스튜디오 (Dataset Studio)
- **스프레드시트 에디터**: Prompt와 Response 쌍을 행 단위로 추가, 편집, 삭제할 수 있는 고성능 웹 그리드를 제공합니다.
- **특화 데이터셋 템플릿**: *고객 지원*, *코드 어시스턴트*, *SQL 전문가*, *창의적 글쓰기* 등 개발 필수 템플릿 기본 탑재.
- **AI 합성 데이터 생성기(Synthetic Generator)**: 설정된 API 키(Gemini, OpenAI, OpenRouter, Claude, Cohere)를 사용해 원하는 주제의 SFT 파인튜닝 데이터셋을 인공지능이 자동 생성하고 빌더에 즉시 추가합니다.
- **통계 및 진단**: 데이터셋 행 수, 총 어휘량(단어 수), 평균 프롬프트 길이 분석 및 형식 오류 자동 검증.

### 4. 🎛️ 파인튜닝 및 PEFT/LoRA 머징
- Epoch, Batch Size, Learning Rate 설정 및 LoRA 하이퍼파라미터(Rank $r$, Alpha $\alpha$) 웹 인터페이스 튜닝 지원.
- SFTTrainer/TRL 학습 루프를 로컬 컨테이너 안에서 실행하고 실시간 터미널 로그를 스트리밍합니다.
- 학습 단계별 손실률(Loss) 변화를 시각화하는 동적 SVG 그래프가 실시간으로 갱신됩니다.
- 훈련이 끝나면 생성된 LoRA 어댑터 가중치를 원본 베이스 모델에 원클릭으로 병합(Merge)할 수 있습니다.

### 5. 🎭 통합 API 플레이그라운드 및 아레나
- 로컬에서 파인튜닝된 어댑터 가중치나 원격 API 모델로 간편하게 단일 챗 제너레이션 테스트 수행 가능.
- **Side-by-Side 비교 아레나**: 서로 다른 두 개의 모델(로컬 또는 원격 API)을 한 화면에 띄우고 동일 프롬프트에 대한 응답 속도, 생성 토큰 수, 퀄리티를 직관적으로 상호 비교합니다.

---

## 🚀 빠른 시작 가이드

### 1. 사전 요구사항
- Docker 및 Docker Compose가 설치되어 있어야 합니다.
- (macOS 사용자) Colima 또는 Docker Desktop 실행 필수:
  ```bash
  colima start
  ```

### 2. 구동 방법
저장소를 클론한 뒤 컨테이너 서비스를 올립니다:
```bash
# 볼륨 매핑을 위한 필수 디렉토리 및 파일 생성
mkdir -p models model_output
touch run_history.json active_dataset.jsonl .env

# 빌드 및 백그라운드 구동
docker compose up --build -d
```
정상 실행되면 브라우저에서 아래 주소로 접속합니다:
👉 **`http://localhost:8500`**

---

## ⚙️ 자격 증명 설정 (Configuration)

Lumina Studio는 설정 화면에서 입력받은 API 키들을 로컬 작업 공간의 `.env` 파일에 안전하게 기록하고 동기화합니다:
- `HF_TOKEN`: 비공개 모델 다운로드 및 커스텀 어댑터 업로드를 위한 Hugging Face 토큰.
- `OPENROUTER_API_KEY`, `OPENAI_API_KEY`, `GEMINI_API_KEY`, `ANTHROPIC_API_KEY`, `COHERE_API_KEY`: API 연동 테스트 및 합성 데이터 생성을 위한 각 플랫폼 API 키.

---

## 📂 프로젝트 구조

```
lumina-studio/
├── public/                 # 프리미엄 글래스모피즘 프론트엔드 자산
│   ├── index.html          # 대시보드 UI 레이아웃
│   ├── app.js              # 프론트엔드 애플리케이션 상태 제어, REST API 통신 및 그래프 렌더링
│   └── style.css           # Vanilla CSS 테마 및 미세 애니메이션
├── server.js               # Node.js Express 백엔드 서버 & 프로세스 제어
├── download_model.py       # 멀티 사이트 가중치 파일 다운로더 엔진
├── fine_tune.py            # Hugging Face PEFT/LoRA 파인튜닝 루프 스크립트
├── merge_peft.py           # 어댑터 가중치 병합 유틸리티
├── local_inference.py      # 로컬 모델 서빙 검증 스크립트
├── Dockerfile              # 가벼운 데비안/도커 이미지 명세
└── docker-compose.yml      # 포트 포워딩 및 호스트 볼륨 맵핑 설정
```

---

<br><br>

# 🌌 Lumina Studio (English Version)

> **Lumina Studio** is a premium, containerized AI development workspace designed to streamline local and remote LLM workflows. Running inside a lightweight Docker container, it integrates multi-site model downloading, tabular dataset engineering, AI synthetic data generation, parameter-efficient fine-tuning (PEFT/LoRA), and side-by-side multi-provider API playgrounds.

---

## 🌟 Key Features

### 1. 🐳 Containerized & Portable
- Fully containerized using a clean multi-stage `Dockerfile` and `docker-compose.yml`.
- Cross-platform system telemetry (CPU usage, RAM, disk, Python & PyTorch environment status).
- Standard CPU-only PyTorch setup by default for near-zero cost/compatibility, supporting GPU/MPS accelerator pass-through.

### 2. 📥 Multi-Site Model Downloader
- **Hugging Face Hub**: Snapshot-download entire models or individual config files.
- **Alibaba ModelScope**: Direct SDK download support using `snapshot_download`.
- **Civitai**: Redirect-friendly downloads for Stable Diffusion and image generation models.
- **Direct HTTP/HTTPS URLs**: Stream weights chunk-by-chunk with real-time speed and percentage progress indicators.
- **Presets Catalog**: One-click selection for lightweight models like Qwen-2.5-0.5B, TinyLlama-1.1B, and OPT-125m.

### 3. 📊 Interactive Dataset Studio
- **Spreadsheet Table Editor**: Add, edit, or remove prompt-response SFT training instructions in a responsive tabular grid.
- **Predefined Templates**: Jumpstart training using templates for *Customer Support*, *Code Assistant*, *SQL Expert*, and *Creative Writer*.
- **AI Synthetic Dataset Generator**: Leverage remote LLM keys (Gemini, OpenAI, OpenRouter, Claude, Cohere) to generate synthetic instruction examples on any topic and append them straight into the tabular editor.
- **Diagnostics & Analytics**: Tracks rows, unique words, average prompt length, and verifies dataset format readiness.

### 4. 🎛️ Local Fine-Tuning Studio & PEFT Merging
- Interactive tuning configurator: configure epochs, batch size, learning rate, and LoRA parameters (Rank $r$, Alpha $\alpha$).
- Runs Hugging Face SFTTrainer/TRL training loop locally.
- Real-time terminal log viewer and a dynamic SVG loss graph updating step-by-step.
- One-click merging to fuse LoRA adapter weights back into base models.

### 5. 🎭 Unified Playground & Arena
- Single Chat Mode for local adapters or API endpoints.
- **Side-by-Side Arena**: Query two different models (local or API) simultaneously to compare output speed, token count, and quality side-by-side.

---

## 🚀 Quick Start

### 1. Prerequisites
- Docker & Docker Compose installed.
- (macOS Users) Colima or Docker Desktop running:
  ```bash
  colima start
  ```

### 2. Setup & Run
Clone the repository and spin up the container:
```bash
# Prepare directories & files for volume mapping
mkdir -p models model_output
touch run_history.json active_dataset.jsonl .env

# Build and start services
docker compose up --build -d
```
Access the dashboard in your browser at:
👉 **`http://localhost:8500`**

---

## ⚙️ Configuration

Lumina Studio saves all credentials securely in the local workspace `.env` file. You can configure them via the **Credentials Settings** panel in the UI:
- `HF_TOKEN`: Hugging Face read/write token for accessing private models/uploading adapters.
- `OPENROUTER_API_KEY`, `OPENAI_API_KEY`, `GEMINI_API_KEY`, `ANTHROPIC_API_KEY`, `COHERE_API_KEY`: API keys for playground chat completions and synthetic dataset generation.

---

## 📂 Project Architecture

```
lumina-studio/
├── public/                 # Glassmorphic Front-End UI
│   ├── index.html          # Main HTML Dashboard Layout
│   ├── app.js              # State Machine, REST Fetchers & SVG Charting
│   └── style.css           # Vanilla CSS Theme Tokens & Animations
├── server.js               # Node.js Express Server & Process Spawner
├── download_model.py       # Multi-source Weight Streamer
├── fine_tune.py            # Hugging Face PEFT/LoRA SFT Scriptor
├── merge_peft.py           # PEFT adapter merge helper
├── local_inference.py      # Local model testing script
├── Dockerfile              # Multi-tier container specification
└── docker-compose.yml      # Port mapping & Host volume configurations
```

---

## 🛡️ License
Distributed under the MIT License. See `LICENSE` for more information.
