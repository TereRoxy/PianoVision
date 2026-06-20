# PianoVision: Polyphonic Audio-to-MIDI Transcription System

[![Python](https://img.shields.io/badge/Python-3.11+-3776ab?logo=python&logoColor=white)](https://www.python.org/)
[![Flutter](https://img.shields.io/badge/Flutter-3.11+-02569B?logo=flutter&logoColor=white)](https://flutter.dev/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.10-EE4C2C?logo=pytorch&logoColor=white)](https://pytorch.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![License](https://img.shields.io/badge/License-CC%20BY--NC%204.0-blue.svg)](LICENSE)

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Project Architecture](#project-architecture)
- [Technology Stack](#technology-stack)
- [System Architecture](#system-architecture)
- [Design Patterns](#design-patterns)
- [Core Components](#core-components)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [Project Structure](#project-structure)

---

## Overview

**PianoVision** is a resource-efficient, Proof-of-Concept end-to-end system that automatically transcribes polyphonic piano audio recordings (.wav) into MIDI files. The project combines advanced machine learning with a cross-platform user interface, enabling musicians and researchers to convert acoustic piano performances into digital notation without industrial-scale computational resources.
This project is designed to be lightweight, deployable on consumer-grade hardware, and provides a user-friendly interface for real-time transcription and visualization.
It has been developed as part of a Bachelor's thesis in Computer Science, focusing on the intersection of audio signal processing, deep learning, and software engineering.

### The Challenge

Piano audio transcription is notoriously difficult due to:
- **Polyphonic complexity**: Multiple notes sounding simultaneously with overlapping harmonics
- **Harmonic ambiguity**: Fundamental frequencies and overtones of different notes overlap in the frequency domain
- **Dynamic range**: Piano has 88 keys spanning over 7 octaves with significant amplitude variations
- **Computational efficiency**: Most commercial solutions require expensive GPUs or cloud services

### The Solution

PianoVision achieves **competitive Proof-of-Concept transcription accuracy* (F₁ ≈ 0.83)** with a sub-7M parameter neural network, making it deployable on consumer-grade hardware while maintaining production-grade reliability.

* F₁-score is the best obtained when evaluated on some MAPS dataset piano instruments, which is a standard benchmark for piano transcription. The exact score may vary based on the specific model checkpoint and evaluation subset used. It does not reflect overall real performance across all possible piano recordings and piano instrument timbres.
---

## Key Features

### Backend (ML & Inference)
✅ **Dilated CNN + Bi-GRU architecture** - Efficient polyphonic pitch detection

✅ **Multi-task learning** - Simultaneous frame and onset detection for improved accuracy

✅ **Real-time inference** - Asynchronous job queue with worker pool architecture

✅ **Multi-dataset training** - Domain adaptation across MAESTRO, BCHT, BSDF, STGB datasets

✅ **Production-ready FastAPI server** - RESTful API with WebSocket support for real-time progress

✅ **Docker containerization** - CPU and GPU variants for flexible deployment

### Frontend (User Interface)
✅ **Cross-platform support** - Android, iOS, Web, Windows, macOS, Linux

✅ **Intuitive transcription workflow** - 3-step process: Select --> Configure --> Visualize

✅ **Real-time progress tracking** - WebSocket-based status updates with detailed logs

✅ **Interactive MIDI visualization** - Piano roll display with synchronized scrolling

✅ **Configurable parameters** - Adjust onset/frame thresholds and processing settings

✅ **Persistent settings** - Backend-managed configuration storage

---

## Project Architecture

```
PianoVision/
├── Bachelor_s_Thesis-backend/          # ML backend & inference server
│   ├── models/                          # Neural network architecture
│   │   └── transcription.py             # TranscriptionEngine (D-CRNN)
│   ├── data/                            # Data pipeline & preprocessing
│   │   ├── core.py                      # CQT/Mel spectrogram generation
│   │   ├── process_maps.py              # HDF5 dataset generation
│   │   └── dataset.py                   # PyTorch Dataset wrapper
│   ├── experiments/                     # Training orchestration
│   │   ├── trainer.py                   # Trainer with multi-task loss
│   │   ├── main.py                      # Training entry point
│   │   └── data_utils.py                # Data split utilities
│   ├── server/                          # FastAPI backend (inference)
│   │   ├── main.py                      # FastAPI application
│   │   ├── controllers/                 # REST API endpoints
│   │   ├── services/                    # Model service, worker pool, queue
│   │   └── models/                      # Pydantic schemas
│   ├── eval/                            # Evaluation & metrics
│   │   ├── evaluate.py                  # Evaluator class
│   │   └── evaluate_all.py              # Batch evaluation
│   ├── scripts/                         # CLI utilities
│   ├── config.yaml                      # Configuration
│   └── requirements.txt                 # Python dependencies
│
├── Bachelor_s_Thesis-frontend/          # Flutter mobile/desktop app
│   ├── lib/
│   │   ├── main.dart                    # Application entry point
│   │   ├── services/                    # API client, MIDI parser, file handlers
│   │   ├── controllers/                 # Business logic (transcription, settings)
│   │   ├── models/                      # Data models & DTOs
│   │   └── gui/                         # UI screens & widgets
│   ├── android/                         # Android configuration
│   ├── ios/                             # iOS configuration
│   ├── web/                             # Web configuration
│   ├── windows/                         # Windows configuration
│   └── pubspec.yaml                     # Flutter dependencies
│
└── README.md                            # This file
```

---

## Technology Stack

### Backend Technologies

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **ML Framework** | PyTorch 2.10 | Neural network implementation |
| **Audio Processing** | librosa, torchaudio | Spectrogram/CQT computation |
| **Server** | FastAPI 0.110+ | RESTful API & WebSocket support |
| **ASGI Server** | Uvicorn 0.28+ | Production-grade server |
| **Data Format** | HDF5 | Efficient dataset storage |
| **Music Theory** | music21, pretty_midi | MIDI generation & notation |
| **Containerization** | Docker, Docker Compose | CPU/GPU deployment |

### Frontend Technologies

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Framework** | Flutter 3.11+ | Cross-platform UI development |
| **HTTP Client** | http, dart:io | API communication |
| **WebSocket** | web_socket_channel | Real-time progress updates |
| **File Handling** | file_picker, path_provider | Audio & MIDI file management |
| **MIDI Parsing** | dart_midi_pro | MIDI file analysis & visualization |
| **Environment** | flutter_dotenv | Configuration management |

### Core Dependencies

**Backend:**
```
torch==2.10.0, torchaudio==2.10.0
librosa==0.11.0, soundfile>=0.13.0
fastapi>=0.110.0, uvicorn>=0.28.0
music21==9.9.1, pretty_midi>=0.2.10
numpy>=2.4.2, scipy>=1.15.0, h5py>=3.12.0
```

**Frontend:**
```
Flutter SDK ^3.11.5
http, file_picker, path_provider
web_socket_channel, flutter_dotenv
dart_midi_pro
```

---

## System Architecture

### Data Flow: Training Pipeline

```
Raw Audio (WAV) + MIDI
    ↓
[Preprocessing] (data/process_maps.py)
    - Extract audio segments
    - Parse MIDI ground truth
    - Align audio with MIDI events
    - Bitrate conversion to 16 kHz
    ↓
[HDF5 Storage] (maestro_h5_16k/)
    - Compressed dataset for efficient I/O
    ↓
[Feature Extraction] (data/core.py)
    - CQT Spectrogram (176 bins @ 24 bins/octave)
    - Mel-spectrogram alternative
    - Normalized to (-1, 1) range
    ↓
[Dataset Creation] (experiments/dataset.py)
    - 10-second segments @ 50 FPS
    ↓
[Model Training] (experiments/trainer.py)
    - TranscriptionEngine with multi-task loss
    - Per-frame & per-onset predictions
    ↓
[Checkpoints] (checkpoints_maps/)
    - best_model.pt (production)
    - best_model_f1.pt (alternative)
```

### Data Flow: Inference Pipeline

```
User selects Audio (WAV)
    ↓
[Frontend: Upload] (Flutter app)
    - File picker --> HTTP multipart upload
    - WebSocket connection for progress tracking
    ↓
[Backend: Job Queue] (server/services/queue.py)
    - Job enqueued with metadata
    ↓
[Worker Pool] (server/services/worker.py)
    - Audio preprocessing
    - Feature extraction
    - Model inference
    ↓
[Model Inference] (server/services/model_service.py)
    - Load TranscriptionEngine
    - Per-frame predictions (onset + frame heads)
    - Greedy decoding --> note events
    ↓
[Post-processing]
    - Gating & median filtering
    - Music21 quantization
    - MIDI event generation
    ↓
[Output: MIDI]
    - Result saved to filesystem
    - Frontend notified via WebSocket
    ↓
[Frontend: Visualization] (Flutter app)
    - MIDI loaded into piano roll
    - Interactive display with scrolling
```

### Component Communication

```
┌─────────────────────────────────────────────────────────────┐
│                 Flutter Frontend (Piano Vision)              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ GUI: Transcribe | Visualizer | Settings              │   │
│  └────────────────┬──────────────────────────────────────┘   │
│                   │ HTTP + WebSocket                        │
└───────────────────┼───────────────────────────────────────────┘
                    │
┌───────────────────┼───────────────────────────────────────────┐
│  FastAPI Server (server/main.py)                             │
│  ├─ /transcribe (POST) - Job submission                      │
│  ├─ /settings (GET/POST) - Configuration management          │
│  ├─ /status/{job_id} (WebSocket) - Real-time progress        │
│  └─ /files/{job_id} (GET) - Result download                  │
├─────────────────────────────────────────────────────────────┤
│  Job Queue Service (services/queue.py)                       │
│  └─ In-memory job storage with status tracking               │
├─────────────────────────────────────────────────────────────┤
│  Worker Pool (services/worker.py)                            │
│  ├─ Parallel audio preprocessing                             │
│  ├─ Model inference (TranscriptionEngine)                    │
│  └─ MIDI generation & post-processing                        │
├─────────────────────────────────────────────────────────────┤
│  Model Service (services/model_service.py)                   │
│  └─ Singleton model loader (lazy initialization)             │
├─────────────────────────────────────────────────────────────┤
│  Trained Models (checkpoints_maps/)                          │
│  └─ best_model.pt (7M parameters)                            │
└─────────────────────────────────────────────────────────────┘
```

---

## Design Patterns

### 1. **Singleton Pattern** (Model Service)
**Purpose**: Ensure only one instance of the trained model exists in memory

```python
# server/services/model_service.py
class ModelService:
    _instance = None
    
    @classmethod
    def get_instance(cls):
        if cls._instance is None:
            cls._instance = ModelService()
        return cls._instance
```

**Benefits**:
- Prevents duplicate model loading
- Reduces memory overhead
- Thread-safe access to the model

### 2. **Job Queue Pattern** (Async Processing)
**Purpose**: Handle long-running inference tasks without blocking HTTP responses

```python
# Request submitted --> Job enqueued --> Worker processes --> Result ready
# Client polls/subscribes via WebSocket for status updates
```

**Benefits**:
- Non-blocking API responses
- Graceful handling of concurrent requests
- Job persistence and recovery
- Real-time progress tracking

### 3. **Worker Pool Pattern** (Parallel Processing)
**Purpose**: Efficiently process multiple inference jobs simultaneously

```python
# server/services/worker.py
class WorkerPool:
    def __init__(self, num_workers=4):
        self.workers = [Worker() for _ in range(num_workers)]
        self.queue = asyncio.Queue()
```

**Benefits**:
- Horizontal scalability
- Load balancing across CPU cores
- Graceful degradation under heavy load

### 4. **Repository Pattern** (Data Access)
**Purpose**: Abstract data storage and retrieval (settings, job metadata)

```python
# server/repository/settings_repo.py
class SettingsRepository:
    def get_settings(self, user_id: str) -> Settings:
        # Database/file access abstraction
        pass
    
    def save_settings(self, user_id: str, settings: Settings):
        pass
```

**Benefits**:
- Decouples business logic from data storage
- Easy to swap storage backends
- Testable with mock repositories

### 5. **MVC Pattern** (Frontend)
**Purpose**: Separate presentation, business logic, and data models in Flutter

```
Controllers (transcription_controller.dart)
    ↓ (manages state)
Models (DTOs, data classes)
    ↓ (represent data)
GUI (gui/screens/)
    ↓ (renders UI)
Services (api_service.dart, file_service.dart)
    ↓ (communicate with backend)
```

**Benefits**:
- Clean separation of concerns
- Testable controllers
- Reusable models and services

### 6. **Multi-Task Learning Pattern** (Model)
**Purpose**: Train a single model to predict multiple related targets

```python
# models/transcription.py - TranscriptionEngine
class TranscriptionEngine(nn.Module):
    def forward(self, x):
        features = self.backbone(x)
        return {
            'frames': self.frame_head(features),      # Active notes
            'onsets': self.onset_head(features)       # Note starts
        }
```

**Benefits**:
- Improved generalization
- Shared feature representations
- Better handling of long-term dependencies

### 7. **Dependency Injection Pattern** (Configuration)
**Purpose**: Inject configuration into services rather than hardcoding

```python
# config/loader.py
class ConfigLoader:
    @staticmethod
    def load(config_path: str) -> dict:
        with open(config_path) as f:
            return yaml.safe_load(f)

# Usage
cfg = ConfigLoader.load("config.yaml")
model = TranscriptionEngine(cfg)
```

**Benefits**:
- Easy configuration management
- Environment-specific settings
- Testable with different configs

---

## Core Components

### 1. **TranscriptionEngine (Dilated CNN + Bi-GRU)**

**Architecture Overview:**
```
Input (B, 1, T, 176)
    ↓
Backbone (Dilated CNN)
├─ DilatedBlock(1-->32, dilation=1)
├─ MaxPool (1×2)
├─ DilatedBlock(32-->48, dilation=2)
├─ MaxPool (1×2)
├─ DilatedBlock(48-->64, dilation=4)
├─ MaxPool (1×2)
└─ DilatedBlock(64-->92, dilation=8)
    ↓
Reshape & Bi-GRU (hidden=256)
    ↓
Bottleneck Dense (512-->256)
    ↓
Multi-task Heads:
├─ Frame Head (256-->88)     [Active note probability]
└─ Onset Head (256-->88)     [Note start probability]
```

**Why Dilated Convolutions?**
- Exponential receptive field growth without increasing parameters
- Captures multi-scale harmonic relationships
- Efficient computation on CPUs

**Model Stats:**
- **Parameters**: ~7M (vs. 20M+ for industry models)
- **Input**: 176 CQT bins (88 piano keys, 2 bins/semitone)
- **Output**: 88 × 2 predictions per frame
- **Inference**: ~50 FPS @ 16 kHz (real-time capable)

### 2. **Data Pipeline**

**CQT Spectrogram Feature:**
- **Frequency resolution**: 24 bins per octave
- **Frequency range**: 27.5 Hz (A0) to 4186 Hz (C8)
- **Time resolution**: 50 FPS (200ms segments)
- **Normalization**: Standardized to (-1, 1) range
- **Alternative**: Mel-spectrogram with same resolution

**Dataset Structure (HDF5):**
```
maestro_h5_16k/
├── {year}/
│   ├── {composition}.h5
│   │   ├── /audio (waveform)
│   │   ├── /ground_truth (MIDI transcription)
│   │   └── /metadata (sample rate, duration)
```

### 3. **FastAPI Backend Server**

**Key Endpoints:**

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/transcribe` | POST | Submit audio for transcription |
| `/status/{job_id}` | WebSocket | Real-time job status |
| `/settings` | GET/POST | Retrieve/update inference settings |
| `/files/{job_id}` | GET | Download resulting MIDI file |
| `/docs` | GET | Interactive API documentation |

**Features:**
- Automatic OpenAPI/Swagger documentation
- CORS support for cross-origin requests
- Multipart file upload with progress tracking
- WebSocket-based real-time updates
- Graceful shutdown with cleanup

### 4. **Flutter Frontend Application**

**Key Screens:**

1. **Transcribe Screen**
   - Audio file picker (WAV format)
   - Real-time upload progress
   - Job queue visualization
   - Results browsing

2. **Visualizer Screen**
   - Interactive piano roll
   - Synchronized horizontal/vertical scrolling
   - Piano key reference
   - Playback controls (future)

3. **Settings Screen**
   - Onset threshold (0-1)
   - Frame threshold (0-1)
   - FPS (frames per second)
   - Segment duration (seconds)
   - Reset to defaults button

**Services:**
- `api_service.dart`: HTTP/WebSocket communication
- `file_service.dart`: Audio/MIDI file handling
- `midi_parser_service.dart`: MIDI file parsing and analysis

---

## Installation & Setup

### Backend Setup

**Prerequisites:**
- Python 3.11+
- Git
- Docker (optional, for containerized deployment)

**Installation:**
```bash
cd Bachelor_s_Thesis-backend

# Create virtual environment
python -m venv venv
source venv/Scripts/activate  # Windows
# or: source venv/bin/activate  # Linux/macOS

# Install dependencies
pip install -r requirements.txt

# Configure settings
# Edit config.yaml as needed
```

**Docker Setup (GPU):**
```bash
docker-compose -f docker-compose.gpu.yaml up
```

### Frontend Setup

**Prerequisites:**
- Flutter SDK 3.11+
- Android SDK / Xcode (for mobile development)
- VS Code or Android Studio

**Installation:**
```bash
cd Bachelor_s_Thesis-frontend

# Get Flutter dependencies
flutter pub get

# Create .env file with backend URL
echo "API_URL=http://localhost:8000" > .env
```

---

## Usage

### Running the Backend Server

```bash
cd Bachelor_s_Thesis-backend

# Development mode
python -m uvicorn server.main:app --reload --host 0.0.0.0 --port 8000

# Production mode (with Gunicorn workers)
gunicorn server.main:app --workers 4 --worker-class uvicorn.workers.UvicornWorker
```

### Running the Frontend

```bash
cd Bachelor_s_Thesis-frontend

# Run on Android emulator
flutter run

# Run on Windows
flutter run -d windows

# Build for release
flutter build apk       # Android
flutter build ipa       # iOS
flutter build windows   # Windows
```

### Training a New Model

```bash
cd Bachelor_s_Thesis-backend

# Configure training in config.yaml
python scripts/run_experiment.py
```

### Evaluating Model Performance

```bash
cd Bachelor_s_Thesis-backend

# Evaluate on test set
python -m eval.evaluate_all
```

---

## Project Structure

```
Bachelor_s_Thesis-backend/
├── config.yaml                          # Configuration file
├── requirements.txt                     # Python dependencies
├── Dockerfile                           # Container definition
├── docker-compose.yaml                  # CPU deployment
├── docker-compose.gpu.yaml              # GPU deployment
│
├── models/
│   ├── __init__.py
│   └── transcription.py                 # TranscriptionEngine (D-CRNN)
│
├── data/
│   ├── __init__.py
│   ├── core.py                          # CQT/Mel spectrogram
│   ├── process_maps.py                  # HDF5 generation
│   ├── extract_category_h5.py           # Dataset extraction
│   ├── preprocess_h5.py                 # Preprocessing utilities
│   └── split_maps_h5.py                 # Train/val/test split
│
├── experiments/
│   ├── __init__.py
│   ├── trainer.py                       # Trainer with loss functions
│   ├── dataset.py                       # PyTorch Dataset
│   ├── main.py                          # Training entry point
│   ├── train.py                         # Training loop
│   └── data_utils.py                    # Data utilities
│
├── server/
│   ├── __init__.py
│   ├── main.py                          # FastAPI application
│   ├── deps.py                          # Dependency injection
│   ├── controllers/
│   │   └── api.py                       # API endpoints
│   ├── services/
│   │   ├── model_service.py             # Model loader (singleton)
│   │   ├── worker.py                    # Worker pool
│   │   ├── queue.py                     # Job queue
│   │   ├── cleanup.py                   # Resource cleanup
│   │   └── transcription_service.py     # Transcription logic
│   ├── models/
│   │   ├── request.py                   # Request schemas
│   │   └── response.py                  # Response schemas
│   └── repository/
│       ├── settings_repo.py             # Settings storage
│       └── job_repo.py                  # Job metadata storage
│
├── eval/
│   ├── __init__.py
│   ├── evaluate.py                      # Evaluator class
│   └── evaluate_all.py                  # Batch evaluation
│
├── scripts/
│   ├── __init__.py
│   ├── run_experiment.py                # Training entry point
│   ├── run_maps_experiment.py           # Training with maps
│   ├── run_with_maps.py                 # Inference with maps
│   ├── aggregate_artefacts.py           # Results aggregation
│   ├── plot_loss.py                     # Loss visualization
│   └── backup_checkpoints_maps.ps1      # Checkpoint backup
│
├── config/
│   ├── __init__.py
│   └── loader.py                        # Config loading
│
├── tests/
│   ├── conftest.py                      # Pytest configuration
│   ├── test_api.py                      # API endpoint tests
│   ├── test_benchmark_inference.py      # Performance tests
│   ├── test_models_job_validation.py    # Model validation
│   ├── test_repos_job_repo.py           # Repository tests
│   └── test_services_model_service.py   # Service tests
│
├── visualization/
│   ├── __init__.py
│   ├── core.py                          # Visualization utilities
│   └── show_heatmap.py                  # Heatmap display
│
├── checkpoints_maps/
│   ├── best_model.pt                    # Production model
│   ├── best_model_f1.pt                 # Alternative model
│   └── last_model.pt                    # Latest checkpoint
│
└── results/
    ├── artefacts/                       # Evaluation artifacts
    ├── artefacts_bsdf/                  # BSDF dataset results
    ├── artefacts_stgb/                  # STGB dataset results
    └── comparison_outputs/              # Model comparisons

Bachelor_s_Thesis-frontend/
├── pubspec.yaml                         # Flutter dependencies
├── lib/
│   ├── main.dart                        # Application entry point
│   ├── barrel_exports.dart              # Dart imports barrel file
│   │
│   ├── controllers/
│   │   ├── transcription_controller.dart
│   │   └── settings_controller.dart
│   │
│   ├── services/
│   │   ├── api_service.dart             # HTTP/WebSocket client
│   │   ├── file_service.dart            # File I/O
│   │   ├── midi_parser_service.dart     # MIDI parsing
│   │   └── settings_service.dart        # Settings management
│   │
│   ├── models/
│   │   ├── transcription_job.dart
│   │   ├── midi_event.dart
│   │   └── app_settings.dart
│   │
│   └── gui/
│       ├── app_shell.dart               # Main application shell
│       ├── screens/
│       │   ├── transcribe_screen.dart
│       │   ├── visualizer_screen.dart
│       │   └── settings_screen.dart
│       └── widgets/
│           ├── piano_roll.dart          # MIDI visualization
│           └── custom_widgets.dart
│
├── android/                             # Android platform code
├── ios/                                 # iOS platform code
├── web/                                 # Web platform code
├── windows/                             # Windows platform code
└── test/
    ├── api_service_test.dart
    ├── midi_parser_service_test.dart
    └── transcription_controller_test.dart
```

---

## Key Achievements

 **Model Efficiency**
- Sub-7M parameters (vs. 20M+ industry standard)
- Real-time inference @ 50 FPS on CPU
- ~83% F1-score on polyphonic piano transcription

 **Production Ready**
- Containerized deployment (CPU & GPU variants)
- Async job queue with worker pool
- WebSocket real-time progress tracking
- Comprehensive error handling

 **User Experience**
- Cross-platform (6+ platforms supported)
- Intuitive 3-step workflow
- Interactive MIDI visualization
- Configurable inference parameters

 **Research Quality**
- Multi-dataset training & evaluation
- Domain adaptation without catastrophic forgetting
- Detailed evaluation metrics per pitch class
- Reproducible training pipeline

---

## Future Enhancements

- [ ] MIDI playback with audio synchronization
- [ ] Batch processing for multiple files
- [ ] Cloud deployment templates (AWS, GCP, Azure)
- [ ] Advanced visualization (spectral view, waveform overlay)
- [ ] Model compression (quantization, pruning)
- [ ] Support for additional instruments
- [ ] Real-time audio input from microphone
- [ ] Web UI for server management
- [ ] User authentication, personalized settings, history tracking and rate limiting for multi-user environments
- [ ] Model training on additional datasets (e.g., full MAPS, MAESTRO v3)
- [ ] Hyperparameter tuning and architecture search for improved accuracy
- [ ] Musicological analysis features (key detection, tempo estimation, chord recognition)
- [ ] Music Symbolic output (MusicXML, LilyPond) in addition to MIDI
- [ ] PDF score generation from MIDI output
- [ ] Personal archive of transcriptions with metadata (date, settings)
- [ ] Multiple audio formats support (MP3, FLAC, OGG)
- [ ] Integration with youtube-dl for direct audio extraction from YouTube links 
- [ ] Horizontal scaling of the model for larger/smaller architectures based on user needs
- [ ] Horizontal scaling of the worker pool for handling varying loads

---

## License

This project is licensed under the **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)** License.

### Summary:
- ✅ **Share & Adapt**: You are free to share and modify this work
- ✅ **Educational Use**: Explicitly permitted for learning and research
- ❌ **Commercial Use**: Not permitted without explicit permission
- ✅ **Attribution**: You must give appropriate credit
- ✅ **As-Is**: No warranties provided; use at your own risk

See [LICENSE](LICENSE) for the full legal text.

---

## Related Work & References

This system builds on foundational research in:
- **Music Information Retrieval (MIR)**: Polyphonic pitch tracking, onset detection
- **Deep Learning**: Dilated convolutions, Bi-GRU architectures, multi-task learning
- **Audio Signal Processing**: CQT spectrograms, STFT analysis
- **MIDI Standards**: Note timing quantization, event scheduling

---

## Contact & Support

For questions, bug reports, or feature requests, please open an issue on the GitHub repository.

---

Author: Roxana Terebent

Note: The full code implementation, thesis, results and related materials will be made publicly available upon completion of the Bachelor's thesis and after the defense date.

**Happy transcribing!**
