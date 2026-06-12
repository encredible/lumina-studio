# 🌌 Lumina Studio

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
