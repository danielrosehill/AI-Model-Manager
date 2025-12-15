# AI Model Manager

A centralized desktop application for managing local AI models across different tools and frameworks.

## The Problem

Anyone using local AI models knows the pain: models scattered everywhere. Ollama stores weights in one place, ComfyUI in another, LM Studio has its own cache, Kdenlive downloads Whisper to yet another location, and HuggingFace has its own cache directory. You end up with:

- **Duplicated models** consuming disk space (the same Whisper model downloaded 3 times)
- **No unified view** of what you actually have installed
- **Confusion** when connecting tools to models ("where did I put that?")
- **Difficulty cleaning up** old or unused models

## The Solution

AI Model Manager provides a **single source of truth** for all your local AI models:

- One base directory for all models, organized by category
- A GUI to browse, search, and manage your model library
- Support for pointing any compatible tool to your centralized store
- Metadata tracking via SQLite database
- Hugging Face integration for downloads

## Features

### Core Features

- **Centralized Storage**: Define a base path (e.g., `~/AI/models`) as your single model store
- **Category-Based Organization**: Models organized by type (LLMs, diffusion, STT, TTS, etc.)
- **GUI Interface**: Desktop application for browsing and managing models
- **SQLite Database**: Track model metadata, locations, and tags
- **Hugging Face Integration**: Download models directly with HF token support

### Model Categories

| Category | Description | Examples |
|----------|-------------|----------|
| `llm/` | Large Language Models | Llama, Mistral, Qwen |
| `llm/gguf/` | Quantized LLMs | GGUF format models |
| `llm/finetunes/` | Fine-tuned LLMs | Custom trained models |
| `embedding/` | Embedding Models | nomic-embed, bge |
| `diffusion/` | Image Generation | Stable Diffusion, FLUX |
| `diffusion/lora/` | LoRA Adapters | Style/character LoRAs |
| `diffusion/controlnet/` | ControlNet Models | Pose, depth, canny |
| `diffusion/vae/` | VAE Models | SD VAEs |
| `stt/` | Speech-to-Text | Whisper, Whisper fine-tunes |
| `tts/` | Text-to-Speech | Fish Speech, Kokoro |
| `multimodal/` | Vision-Language Models | LLaVA, Qwen-VL |
| `misc/` | Other Models | Punctuation restoration, etc. |

### Planned Features

- **Duplicate Detection**: Find and remove duplicate models
- **AI Assistant**: Chat with a local agent to help clean up and organize
- **Import Wizard**: Scan existing tool caches and consolidate
- **Export/Sync**: Backup model library metadata

## Installation

*Coming soon*

## Usage

### Setting Up Your Model Store

1. Launch AI Model Manager
2. Configure your base path (e.g., `~/AI/models`)
3. The app creates the category folder structure
4. Add your Hugging Face token for authenticated downloads

### Pointing Tools to Your Store

Most AI tools support custom model paths:

**Ollama:**
```bash
export OLLAMA_MODELS=~/AI/models/llm
```

**ComfyUI:**
```yaml
# extra_model_paths.yaml
base_path: ~/AI/models/diffusion
```

**LM Studio:**
Configure model directory in settings to `~/AI/models/llm/gguf`

## Tech Stack

*To be determined* - Candidates:
- **GUI**: Electron, Tauri, PyQt, or web-based (Flask/FastAPI + browser)
- **Database**: SQLite
- **Language**: Python or TypeScript

## Contributing

Contributions welcome! This project is open source.

## License

*TBD*

## Acknowledgments

Inspired by the organizational approach of ComfyUI's model directory structure and tools like [OllaMan](https://ollaman.com) and [civitai-data-manager](https://github.com/jeremysltn/civitai-data-manager).
