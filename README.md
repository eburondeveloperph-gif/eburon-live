# Eburon

**Live speech translation for desktop, browser & mobile.**

Eburon translates speech in real-time using on-device AI or cloud providers. It runs as an Electron desktop app (Windows, macOS, Linux), a Chrome/Edge browser extension, and an Android app — all from a single codebase.

[![Build](https://github.com/eburondeveloperph-gif/eburon-live/actions/workflows/build.yml/badge.svg)](https://github.com/eburondeveloperph-gif/eburon-live/actions/workflows/build.yml)
![License](https://img.shields.io/badge/license-Proprietary-red?style=flat-square)

---

## Features

### Local Inference (Edge AI)
- 48 ASR models (99+ languages) via sherpa-onnx WASM & Whisper WebGPU
- 55+ translation language pairs (Opus-MT) + Qwen 2.5/3/3.5 multilingual LLMs
- 136 TTS models across 53 languages (Piper, Coqui, Mimic3, Matcha)
- One-click model download, IndexedDB caching, CPU (WASM) or GPU (WebGPU)

### Audio
- Virtual microphone output (VB-Cable / macOS virtual driver / PulseAudio)
- System audio capture from video calls
- Real-time voice passthrough with adjustable volume
- Dual-queue audio mixing (regular + immediate tracks)
- Push-to-Talk mode

### Platforms
| Platform | Format | Method |
|----------|--------|--------|
| Windows | `.exe` | Squirrel installer |
| macOS | `.pkg` | Unsigned PKG |
| Linux | `.deb` | Debian package |
| Android | `.apk` | Capacitor |
| Browser | Extension | Chrome/Edge (Manifest V3) |

### Supported Video Conferencing
Google Meet · Microsoft Teams · Zoom · Discord · Slack · Gather.town · Whereby

---

## Quick Start

### From Release

Download the latest build for your platform from [Releases](https://github.com/eburondeveloperph-gif/eburon-live/releases).

### From Source

```bash
git clone https://github.com/eburondeveloperph-gif/eburon-live.git
cd eburon-live
npm install
npm run electron:dev     # Desktop (Electron)
npm run dev              # Web / extension dev
```

### Build All Platforms

```bash
./build-all.sh              # Build everything
./build-all.sh mac          # macOS PKG only
./build-all.sh debian       # Debian .deb only
./build-all.sh windows      # Windows .exe only
./build-all.sh apk          # Android APK only
```

Or use npm scripts:
```bash
npm run make:all
npm run make:mac
npm run make:deb
npm run make:win
npm run make:apk
```

---

## Development

| Command | Description |
|---------|-------------|
| `npm run electron:dev` | Electron + Vite dev server |
| `npm run dev` | React only (extension/web dev) |
| `npm run test` | Run Vitest tests |
| `npm run extension:dev` | Extension build in watch mode |
| `npm run make` | Build + package Electron distributable |
| `npm run eval` | Run LLM-as-Judge eval suite |

### Project Structure

```
src/            React components, stores, services (shared across all platforms)
electron/       Electron main process (JS)
extension/      Chrome extension: service worker, content scripts
android/        Capacitor Android project
evals/          LLM-as-Judge translation quality evaluation
model-packs/    ASR & TTS model definitions and browser demos
```

### Architecture

```
Input Device → ModernAudioRecorder → AI Provider Client → ModernAudioPlayer → Output Device
```

| Layer | Location | Pattern |
|-------|----------|---------|
| State | `src/stores/` | Zustand with `subscribeWithSelector` |
| Services | `src/services/` | `ServiceFactory` — singleton, platform-aware |
| AI Clients | `src/services/clients/` | `ClientFactory` → `IClient` implementations |
| Audio | `src/lib/modern-audio/` | `BaseAudioRecorder` hierarchy, queue-based playback |
| Platform | `src/utils/environment.ts` | `isElectron()`, `isExtension()`, `isWeb()` |

### Tech Stack

- **Runtime**: Electron 40+ / Chrome Extension Manifest V3 / Capacitor
- **Frontend**: React 18, TypeScript, Zustand, SASS
- **Local AI**: sherpa-onnx (WASM), @huggingface/transformers, WebGPU
- **Protocols**: WebSocket, WebRTC, protobuf
- **i18n**: 30 languages via i18next

---

## License

Proprietary — see [LICENSE](LICENSE).

Copyright © 2026 Eburon. All Rights Reserved.
