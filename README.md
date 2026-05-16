# AEGIS — Autonomous Embedded General Intelligence System

> A modular, locally-run personal AI assistant with persistent memory, emotional modeling, self-training, and desktop integration. Your own Jarvis — running entirely on your machine.

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![LLM](https://img.shields.io/badge/LLM-Mistral%20via%20Ollama-purple)
![Memory](https://img.shields.io/badge/Memory-ChromaDB%20%2B%20NetworkX-orange)
![GUI](https://img.shields.io/badge/GUI-PySide6-green)
![Status](https://img.shields.io/badge/Status-In%20Development-yellow)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

---

## What is AEGIS?

Most AI assistants live in the cloud, forget everything between sessions, and have no real personality. **AEGIS** is different.

AEGIS is a fully local, self-contained AI assistant framework built around **AEGIS** (the core intelligence layer) — designed to run on your own desktop, remember your conversations, adapt its personality, and eventually improve itself over time.

```
You talk to AEGIS
  ↓
She remembers you (ChromaDB + Knowledge Graph)
  ↓
She responds with context, emotion, and personality
  ↓
She trains herself at 2am while you sleep
```

Think of it as building your own personal AI — one that actually knows who you are.

---

## Core Architecture

AEGIS is split into focused, independent modules:

| Module | Role |
|---|---|
| `aegis_core` | Kernel, memory, security, emotion & personality engines |
| `aegis_ml` | Chat engine, LLM backends, code editor |
| `aegis_desktop` | PySide6 GUI — chat interface, dashboard, controls |
| `aegis_training` | Autonomous self-training pipeline (scheduled) |
| `aegis_memory` | ChromaDB persistent vector store |
| `config/` | YAML-driven configuration — no code changes needed |

---

## Features

**Memory System**
- Vector memory via ChromaDB (384-dimension embeddings, up to 100,000 entries)
- Relational knowledge graph via NetworkX
- Semantic similarity search with configurable threshold
- Persistent across sessions — HELENA remembers past conversations

**Emotion Engine**
- 8 emotions: Curiosity, Satisfaction, Frustration, Concern, Enthusiasm, Calm, Determination, Empathy
- Emotions decay toward baseline over time — not static flags
- Dominant emotion is injected into every response's context

**Personality Engine**
- Tunable parameters: verbosity, humor, technical depth, creativity, formality
- Response style: `concise_technical` by default
- Humor style: `dry_technical`

**LLM Backend (HybridLLM)**
- Priority chain: Ollama (Mistral) → LocalLLM (GGUF) → SimpleFallback
- Entirely offline-capable — no OpenAI API key needed
- Designed to slot in AEGIS's own fine-tuned model when ready

**Self-Training Pipeline**
- Scheduled: daily at 2am, weekly deep run on Sundays
- Tracks model evolution in SQLite
- Sandboxed code execution for safe testing
- Stubs in place for Phase 3: patch application, response refinement, feedback collection

**Safety & Security**
- Emergency kill switch (`kill_switch.py`) — protected, never auto-modified
- Protected files list — core files cannot be self-edited
- Explicit block on autonomous self-upgrades
- Regulatory compliance checks on every task
- Permission system: mode × source × command before execution

**Operational Modes**

| Mode | Code Gen | System Control | Memory Write | Chat |
|---|---|---|---|---|
| ENGINEERING *(default)* | ✓ | ✓ | ✓ | ✓ |
| TOOL | ✓ | ✗ | ✗ | ✓ |
| DEFENSIVE | ✗ | ✓ | ✓ | ✗ |
| BACKGROUND | ✗ | ✗ | ✗ | ✗ |

**Desktop Integration**
- PySide6 GUI with chat interface, dashboard, and controls panel
- Console tab for direct system access
- Gaming mode — throttles resource usage when games are detected

---

## Project Structure

```
AEGIS/
├── start_helena.py              # Entry point — launches the desktop app
├── config.yaml                  # Runtime configuration
├── config.default.yaml          # Default config (do not edit)
│
├── aegis_core/                  # Core systems
│   ├── kernel/
│   │   ├── core.py              # AEGISKernel — central authority  [PROTECTED]
│   │   ├── modes.py             # ModeProcessor — routes tasks
│   │   ├── emotion.py           # EmotionEngine — 8 emotions with decay
│   │   ├── personality.py       # PersonalityEngine + ResponseFormatter
│   │   ├── validation.py        # ValidationChain
│   │   └── regulatory.py        # Regulatory compliance checks
│   ├── memory/
│   │   ├── vector_store.py      # ChromaDB wrapper
│   │   └── graph_memory.py      # NetworkX knowledge graph
│   ├── runtime/
│   │   ├── profiles.py          # Hardware profiles (85% CPU/RAM cap)
│   │   └── resources.py         # Resource monitor
│   └── security/
│       ├── kill_switch.py       # Emergency shutdown  [PROTECTED]
│       └── encryption.py        # EncryptionManager
│
├── aegis_ml/                    # Machine learning layer
│   ├── chat_engine.py           # Main conversation loop  [CRITICAL]
│   ├── llm.py                   # HybridLLM — Ollama/GGUF/fallback chain
│   ├── code_editor.py           # Safe read/write of own source
│   └── speech.py                # Speech (stub)
│
├── aegis_desktop/               # PySide6 GUI
│   ├── main_window.py           # MainWindow + system wiring
│   ├── chat_interface.py        # Chat UI + ChatWorker thread
│   ├── dashboard.py             # System dashboard
│   └── console_interface.py     # Console tab
│
├── aegis_training/              # Autonomous training pipeline
│   ├── trainer.py               # AutonomousTrainer orchestrator
│   ├── scheduler.py             # Training scheduler ✓
│   ├── dataset.py               # Dataset management ✓
│   ├── evolution.py             # Evolution tracking (SQLite) ✓
│   ├── sandbox.py               # Sandboxed code execution ✓
│   └── [integration, refinement, feedback, auditor — Phase 3]
│
├── aegis_memory/                # ChromaDB persistent storage (auto-created)
│   └── chroma.sqlite3
│
├── config/                      # YAML configuration files
├── docs/                        # Documentation
├── examples/                    # Example scripts
├── tests/                       # Test suite
├── scripts/                     # Utility scripts
├── training/                    # Training data
└── logs/                        # Runtime logs
    ├── system.log
    ├── user.log
    └── audit.log
```

---

## Quickstart

### 1. Prerequisites

- Python 3.10+
- [Ollama](https://ollama.com) installed and running with Mistral pulled:

```bash
ollama pull mistral
```

### 2. Clone and install

```bash
git clone https://github.com/ANSHRAMANI1/AEGIS.git
cd AEGIS
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### 3. Configure

```bash
cp config.default.yaml config.yaml
# Edit config.yaml to set your memory path, hardware limits, etc.
```

### 4. Launch

```bash
# Full desktop GUI
python start_helena.py

# Or with console mode (no GUI)
python start_helena.py --console
```

---

## How It Works

```
User types message in GUI
        ↓
  ChatInterface.send_message()
        ↓
  ChatWorker (background thread)
        ↓
  HELENAKernel.submit_task("chat", message)
        ↓
  ValidationChain  →  Permission check  →  ModeProcessor
        ↓
  ChatEngine.chat(message)
        ├── Detect tool intent (code read/write?)
        ├── Classify intent
        ├── Retrieve emotion state
        ├── Search memory (ChromaDB)
        ├── Build system prompt (identity + emotion + personality + memory)
        └── HybridLLM.chat(messages)   ← Mistral via Ollama
        ↓
  PersonalityEngine.apply()  →  ResponseFormatter.format()
        ↓
  Response rendered in GUI
```

---

## Configuration (YAML)

All behavior is controlled via `config.yaml` — no code changes needed for most tuning.

```yaml
system:
  name: "AEGIS"
  operator: "Phase-Null"
  mode: "ENGINEERING"

personality:
  verbosity: 0.4
  technical_depth: 0.8
  humor_frequency: 0.7
  formality: 0.8
  response_style: "concise_technical"

memory:
  storage_path: "./aegis_memory"
  vector_dimension: 384
  max_entries: 100000
  search_threshold: 0.6

hardware:
  cpu_limit_percent: 85.0
  ram_limit_percent: 85.0
  gaming_mode: true          # Throttles when games are detected

training:
  schedule_daily: "02:00"
  schedule_weekly: "Sunday"
```

---

## Roadmap

| Phase | Status | Description |
|---|---|---|
| Phase 1 | ✅ Complete | Core kernel, memory, emotion, GUI |
| Phase 2 | ✅ Complete | Chat engine, HybridLLM, training scheduler |
| Phase 3 | 🔄 In Progress | Structured FactStore, training integration, response refinement |
| Phase 4 | 📋 Planned | AEGIS's own fine-tuned model, voice interface |

### Known Limitations

| Issue | Severity | ETA |
|---|---|---|
| Cross-session memory unreliable — fuzzy ChromaDB matches | Medium | Phase 3 |
| Training stubs not yet implemented | Medium | Phase 3 |
| Speech input is a stub | Low | Phase 4 |

---

## Tech Stack

| Component | Technology |
|---|---|
| Language | Python 3.10+ / Rust (performance modules) |
| LLM | Mistral via Ollama (local), GGUF fallback |
| Memory | ChromaDB (vector) + NetworkX (graph) |
| GUI | PySide6 |
| Training Store | SQLite |
| Config | YAML |
| Embeddings | Offline bag-of-words (384-dim, no download) |

---

## Security Model

AEGIS is built with safety constraints embedded at the architecture level:

- **Kill switch** — immediate shutdown, never auto-modified
- **Protected files** — `core.py`, `kill_switch.py`, `start_helena.py` cannot be self-edited
- **No autonomous self-upgrade** — AEGIS can read and propose edits, but not deploy to GitHub
- **Sandboxed training** — all self-generated code runs in an isolated sandbox before any patch is considered
- **Permission system** — every task validated against mode × source × command matrix

---

## Contributing

This is an experimental passion project, currently in active development. Contributions, ideas, and issues are welcome.

1. Fork the repo
2. Create a feature branch: `git checkout -b feature/your-idea`
3. Commit your changes
4. Open a pull request

---

## License

MIT — see [LICENSE](LICENSE) for details.

---

*AEGIS is a personal research project — not production software. Built by [ANSHRAMANI1](https://github.com/ANSHRAMANI1).*

**Interested in a custom local AI assistant or agent framework?** [Hire me on Upwork](https://www.upwork.com/freelancers/~0169dbb8a7f7cf38e6)
