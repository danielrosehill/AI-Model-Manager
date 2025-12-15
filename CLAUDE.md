# CLAUDE.md - AI Model Manager

## Project Overview

AI Model Manager is a desktop application for centralized management of local AI models across different tools and frameworks. The goal is to solve the fragmentation problem where models are scattered across Ollama, ComfyUI, LM Studio, HuggingFace cache, and various other locations.

## Project Status

**Phase**: Early specification / pre-development

The project currently contains:
- Specification documents
- Original voice notes (in `notes/`)
- No implementation code yet

## Architecture Decisions (Pending)

### GUI Framework Options

| Option | Pros | Cons |
|--------|------|------|
| **Tauri** | Small binary, Rust backend, web frontend | Requires Rust knowledge |
| **Electron** | Familiar web stack, large ecosystem | Heavy, resource-intensive |
| **PyQt/PySide** | Native feel, Python backend | Licensing considerations |
| **Web UI (Flask/FastAPI)** | Simple, browser-based | Not a "true" desktop app |

### Database

SQLite is the planned choice for:
- Model metadata storage
- Category/tag management
- Path tracking

### Language

Not yet decided. Candidates:
- **Python**: Familiar to AI/ML users, easy HuggingFace integration
- **TypeScript**: If using Electron/web approach
- **Rust + TypeScript**: If using Tauri

## Key Concepts

### Base Path
User-configurable root directory for all models (e.g., `~/AI/models`)

### Categories
Hierarchical folder structure:
```
base_path/
├── llm/
│   ├── gguf/
│   └── finetunes/
├── embedding/
├── diffusion/
│   ├── checkpoints/
│   ├── lora/
│   ├── controlnet/
│   └── vae/
├── stt/
├── tts/
├── multimodal/
└── misc/
```

### Model Entry
Database record containing:
- File path (relative to base)
- Category
- Tags (e.g., "gguf", "q4_k_m", "finetune")
- Source (HuggingFace, Civitai, manual)
- File size
- Hash (for duplicate detection)
- Added date
- Last used date (optional)

## Development Guidelines

### When Implementing

1. **Start simple**: MVP is a GUI that displays models in the configured directory
2. **SQLite first**: Get the database schema right before adding features
3. **Category structure**: Follow ComfyUI's model organization as inspiration
4. **HuggingFace integration**: Use `huggingface_hub` Python library

### File Formats to Support

| Format | Used By | Category |
|--------|---------|----------|
| `.gguf` | llama.cpp, Ollama, LM Studio | LLM |
| `.safetensors` | ComfyUI, A1111, InvokeAI | Diffusion/LLM |
| `.bin` | HuggingFace transformers | Various |
| `.pt`, `.pth` | PyTorch models | Various |
| `.onnx` | ONNX runtime | Various |

### External Tool Integration

Document how to point each tool to the centralized store:
- Ollama: `OLLAMA_MODELS` env var
- ComfyUI: `extra_model_paths.yaml`
- LM Studio: Settings UI
- HuggingFace: `HF_HOME` env var
- InvokeAI: `invokeai.yaml` config

## Testing

When implementation begins:
- Test with real model files (large files)
- Test category detection accuracy
- Test duplicate detection (hash comparison)
- Test HuggingFace download integration

## Related Resources

- [HuggingFace Hub caching docs](https://huggingface.co/docs/huggingface_hub/en/guides/manage-cache)
- [ComfyUI model organization](https://docs.comfy.org/development/core-concepts/models)
- [OllaMan](https://ollaman.com) - Similar tool for Ollama only
- [civitai-data-manager](https://github.com/jeremysltn/civitai-data-manager) - Civitai metadata tool

## Commands

*No build/run commands yet - project is in specification phase*
