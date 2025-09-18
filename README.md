# FireRedChat: A Fully Self-Hosted Solution for Full-Duplex Voice Interaction

<div align="center">
  <a href="https://fireredteam.github.io/demos/firered_chat/">Demo</a> •
  <a href="https://arxiv.org/pdf/2509.06502">Paper</a> •
  <a href="https://huggingface.co/FireRedTeam">Huggingface</a>
</div>

<div align="center">
  <a href="#what-is-fireredchat">Overview</a> •
  <a href="#features">Features</a> •
  <a href="#quickstart">Quickstart</a> •
  <a href="#acknowledgements">Acknowledgements</a>
  <br>
  <img alt="License" src="https://img.shields.io/badge/license-Apache%202.0-blue.svg">
</div>

## 🔥 News
- **2025/09/16**: Released models (pVAD, turn-detector) and services (FireRedTTS1, FireRedASR) for the cascade system.

## What is FireRedChat?

FireRedChat offers a fully self-hosted solution for building real-time voice AI agents. It integrates robust **TTS** (Text-to-Speech), **ASR** (Automatic Speech Recognition), **pVAD** (Personalized Voice Activity Detection), and **EoT** (End-of-Turn) functionalities, enabling developers to create customizable, privacy-focused AI agents with:

- **No external APIs required**
- **No information leaks**
- **Full control over deployment**

## Features

| **Module** | **Description** |
|------------|-------------|
| 🆕 **TTS Service** | **Accelerated** FireRedTTS with **text normalization and G2P** support. |
| 🆕 **ASR Service** | Automatic speech recognition with **punctuation model** integration. |
| 🆕 **pVAD** | **Personalized** Voice Activity Detection for improved barge-in experience. |
| 🆕 **Turn-Detector** | **Small** end-of-turn detection for faster response (English and Chinese). |
| 🆕 **Context-Aware TTS** <br> *Coming soon* | **Context-aware** TTS with text normalization and G2P. |
| 🆕 **Audio LLM Service** <br> *Coming soon* | **Acoustically aware** LLM with vLLM acceleration. |
| **Fork of [livekit/agents](https://github.com/livekit/agents)** | Core framework for real-time voice AI agent development. |
| **Fork of [livekit/agents-playground](https://github.com/livekit/agents-playground)** | Simple web UI for user-friendly interaction. |

## Quickstart

Explore the [demo](https://fireredteam.github.io/demos/firered_chat/) or follow the deployment steps below to get started.

### Step 1: Livekit RTC Service Configuration
If you have a domain name, generate configuration files:
```bash
mkdir output
docker pull livekit/generate
docker run --rm -it -v $PWD:/output livekit/generate
```

For local machine development (Redis, RTC, & WebUI):
```bash
cd docker
docker-compose up -d
```

### Step 2: Start Supporting Services

- **ASR Service**:

  [ASR Server README](./fireredasr-server/README.md)

- **TTS Service**:

  [TTS Server README](./fireredtts-server/README.md)

- **Agent Service**:

  [Agent Service README](https://github.com/fireredchat-submodules/agents/blob/fireredchat/README.md)


## Acknowledgements

We extend our gratitude to the following open-source projects:
- [livekit/livekit (RTC)](https://github.com/livekit/livekit)
- [livekit/agents](https://github.com/livekit/agents)
- [livekit/agents-playground](https://github.com/livekit/agents-playground)
- [speechbrain/spkrec-ecapa-voxceleb](https://huggingface.co/speechbrain/spkrec-ecapa-voxceleb)
- [google-bert/bert-base-multilingual-cased](https://huggingface.co/google-bert/bert-base-multilingual-cased)

## Disclaimer
The content provided is for academic purposes only and is intended to demonstrate technical capabilities.
