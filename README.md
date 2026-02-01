# RealtimeVoiceChat

A real-time voice conversation system powered by AI. Speak naturally with an LLM and receive instant voice responses.

## Overview

This application enables natural voice-based conversations with Large Language Models. It captures your voice, transcribes it in real-time, processes it through an LLM, and returns synthesized speech responses with minimal latency.

## Features

- **Real-time voice interaction** - Speak and receive responses naturally
- **Live transcription** - See your words and AI responses as they happen
- **Smart turn detection** - Intelligent silence detection adapts to conversation flow
- **Multiple LLM support** - Works with Ollama (local) and OpenAI
- **Multiple TTS engines** - Choose from Kokoro, Coqui, or Orpheus voices
- **Interruption handling** - Jump in anytime during AI responses
- **Web-based interface** - Access through any modern browser
- **Docker support** - Easy deployment with Docker Compose

## How It Works

1. **Voice Capture** - Browser captures your voice via microphone
2. **Audio Streaming** - Audio chunks sent to backend via WebSockets
3. **Speech-to-Text** - RealtimeSTT converts speech to text
4. **LLM Processing** - Text sent to configured LLM for response
5. **Text-to-Speech** - RealtimeTTS synthesizes voice response
6. **Audio Return** - Generated audio streams back to browser

## Tech Stack

**Backend**
- Python (< 3.13)
- FastAPI
- WebSockets

**AI/ML**
- RealtimeSTT (Speech-to-Text)
- RealtimeTTS (Text-to-Speech)
- Transformers (Turn detection)
- PyTorch / Torchaudio

**LLM Providers**
- Ollama
- OpenAI

**Frontend**
- HTML/CSS/JavaScript
- Web Audio API
- AudioWorklets

**Deployment**
- Docker
- Docker Compose

## Requirements

- **OS**: Windows (install script provided), Linux (Docker recommended), macOS (manual setup)
- **Python**: 3.9 - 3.12
- **GPU**: NVIDIA GPU with CUDA 12.1 recommended for optimal performance
- **Docker**: Optional but recommended for Linux deployment

## Installation

### Option 1: Docker (Recommended for Linux)

```bash
# Build images
docker compose build

# Start services
docker compose up -d

# Pull Ollama model
docker compose exec ollama ollama pull hf.co/bartowski/huihui-ai_Mistral-Small-24B-Instruct-2501-abliterated-GGUF:Q4_K_M
```

### Option 2: Windows Install Script

```batch
install.bat
```

### Option 3: Manual Installation

```bash
# Create virtual environment
python -m venv venv

# Activate (Linux/macOS)
source venv/bin/activate

# Activate (Windows)
.\venv\Scripts\activate

# Navigate to code directory
cd code

# Install PyTorch with CUDA
pip install torch==2.5.1+cu121 torchaudio==2.5.1+cu121 torchvision --index-url https://download.pytorch.org/whl/cu121

# Install dependencies
pip install -r requirements.txt
```

## Usage

### With Docker
Services start automatically with `docker compose up -d`

### Manual Setup
```bash
cd code
python server.py
```

### Access the Application
1. Open browser to `http://localhost:8000`
2. Grant microphone permission
3. Click "Start" to begin conversation
4. Click "Stop" to end, "Reset" to clear history

## Configuration

Configuration files are located in the `code/` directory:

| Setting | File | Description |
|---------|------|-------------|
| TTS Engine | `server.py` | Set `START_ENGINE` to "coqui", "kokoro", or "orpheus" |
| LLM Provider | `server.py` | Set `LLM_START_PROVIDER` to "ollama" or "openai" |
| LLM Model | `server.py` | Set `LLM_START_MODEL` |
| AI Personality | `system_prompt.txt` | Customize AI behavior |
| STT Settings | `transcribe.py` | Whisper model, language, silence thresholds |
| Turn Detection | `turndetect.py` | Pause duration sensitivity |
| SSL/HTTPS | `server.py` | Enable SSL and set certificate paths |

**Note**: For Docker deployments, make configuration changes before running `docker compose build`.

## Project Structure

```
RealtimeVoiceChat/
├── code/
│   ├── server.py              # FastAPI main server
│   ├── audio_module.py        # TTS processing
│   ├── transcribe.py          # STT processing
│   ├── llm_module.py          # LLM integration
│   ├── turndetect.py          # Turn detection logic
│   ├── speech_pipeline_manager.py  # Pipeline orchestration
│   ├── audio_in.py            # Audio input handling
│   ├── text_context.py        # Context management
│   ├── text_similarity.py     # Text comparison utilities
│   ├── upsample_overlap.py    # Audio processing
│   ├── colors.py              # Console colors
│   ├── logsetup.py            # Logging configuration
│   ├── system_prompt.txt      # AI personality prompt
│   └── static/                # Web interface files
├── wheels/                    # Pre-built Python wheels
├── Dockerfile                 # Container definition
├── docker-compose.yml         # Multi-container setup
├── requirements.txt           # Python dependencies
├── install.bat               # Windows installer
└── entrypoint.sh             # Docker entrypoint
```

## Docker Commands

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View app logs
docker compose logs -f app

# View Ollama logs
docker compose logs -f ollama

# List Ollama models
docker compose exec ollama ollama list
```

## Troubleshooting

**Slow performance**: Ensure CUDA-enabled GPU is being used. CPU-only mode is significantly slower.

**Microphone not working**: Check browser permissions and ensure microphone access is granted.

**Connection issues**: Verify WebSocket connection at `ws://localhost:8000/ws`

**Model not found**: For Ollama, ensure the model is pulled: `ollama pull <model-name>`
