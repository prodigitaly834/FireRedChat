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

- **No reliance on external APIs**
- **Zero data leakage**
- **Complete deployment control**

The system architecture includes:
- **LiveKit RTC Server**: Acts as the core video/audio chat meeting room for real-time communication.
- **Agents (AI-Agent Bot Server)**: Handles the intelligent agents that process and respond to user interactions.
- **Agents-Playground (WebUI)**: A user-friendly web interface for joining and interacting with the chat rooms online.
- **Redis Server**: Enables multi-node hosting for scalability and data persistence across instances.
- **TTS Server**: Handles text-to-speech conversion for agent responses.
- **ASR Server**: Handles automatic speech recognition for user inputs.

Additionally, you'll need to host your own LLM (Large Language Model) server to power the AI agents. This can be a simple setup like an Ollama server, vLLM server or a more advanced one, such as Dify, depending on your needs.

## Features

| **Module** | **Description** |
|------------|-------------|
| 🆕 **TTS Service** | **Accelerated** FireRedTTS with **text normalization and G2P** (Grapheme-to-Phoneme) support. |
| 🆕 **ASR Service** | Automatic speech recognition with **punctuation model** integration. |
| 🆕 **pVAD** | **Personalized** Voice Activity Detection for improved barge-in experience. |
| 🆕 **Turn-Detector** | **Compact** end-of-turn detection for quicker response (English and Chinese). |
| 🆕 **Context-Aware TTS** <br> *Coming soon* | **Context-aware** TTS with text normalization and G2P. |
| 🆕 **Audio LLM Service** <br> *Coming soon* | **Acoustically aware** LLM with vLLM acceleration. |
| **Fork of [livekit/agents](https://github.com/livekit/agents)** | Core framework for real-time voice AI agent development. |
| **Fork of [livekit/agents-playground](https://github.com/livekit/agents-playground)** | Intuitive web UI for easy user-interactions. |

## Quickstart

Try the [demo](https://fireredteam.github.io/demos/firered_chat/) or follow these steps to deploy your own instance. Note that you'll need to set up your own LLM server (e.g., Ollama or Dify) to integrate with the AI-Agent Bot Server for full functionality.

### Step 1: Deploy the RTC Server, Redis Server, and WebUI
In this step, we'll set up the foundational services: the LiveKit RTC Server (for real-time communication), the Redis Server (for multi-node support), and the WebUI (for browser-based access).

First, clone the repository:
```bash
git clone --recurse-submodules https://github.com/FireRedTeam/FireRedChat.git
```

(Optional) If you have a domain name, follow the official [doc](https://docs.livekit.io/home/self-hosting/vm/#generate-configuration) and generate your configuration files:
```bash
mkdir output
docker pull livekit/generate
docker run --rm -it -v $PWD:/output livekit/generate
```

To quickly startup RTC Server, Redis Server, and WebUI services on the same machine, use [Docker Compose](https://github.com/docker/compose/releases):
```bash
cd docker
docker-compose up -d
```

Once running:
- The RTC Server will be hosted at `0.0.0.0:7880`. It uses two UDP ports per user for data communication (e.g., audio/video streams).
- The WebUI can be accessed at `0.0.0.0:3000` in your browser, allowing users to join chat rooms online.


#### Secured connection
*Note: Secured connections are not required for local development if accessing from the same machine hosting the LiveKit RTC Server.*

To secure the LiveKit RTC Server connection (upgrading `ws://0.0.0.0:7880` to `wss://`) and serve the WebUI securely via HTTPS, configure Nginx as a reverse proxy. For detailed instructions, refer to the [Foundry VTT LiveKit Hosting Guide](https://foundryvtt.wiki/en/setup/hosting/Self-Hosting-LiveKit-Audio-Video-Server-on-Existing-Linux-Setup).

**Notes**:
- After configuring Nginx, update your LiveKit client to use `wss://your-domain.com/livekit` instead of `ws://0.0.0.0:7880`. The WebUI will be accessible at `https://your-domain.com`.
- Ensure port 443 is open on your firewall for HTTPS/WSS traffic, and the UDP port range (e.g., `50000-60000`) is open for LiveKit media streams.

### Step 2: Start Supporting Services

Launch the additional servers required for voice processing and AI functionality.

- **FireRedASR Service** (Automatic Speech Recognition):

  Refer to the [ASR Server README](./fireredasr-server/README.md) for setup instructions.

- **FireRedTTS Service** (Text-to-Speech):

  Refer to the [TTS Server README](./fireredtts-server/README.md) for setup instructions.

- **LLM Service**
  Deploy a self-hosted LLM server to power the AI agents. Options include [Ollama](https://ollama.com) for a lightweight setup, [vLLM](https://docs.vllm.ai/en/latest/deployment/docker.html) for high-performance inference, or additionally with [Dify](https://github.com/langgenius/dify) for advanced workflows.

### Step 3: Start AI-Agents Service

- **Agents Service** (AI-Agent Bot Server):

  Upon a user joining a chat room, a bot worker is automatically dispatched to handle interactions. This service drives the intelligent agents and depends on the ASR, TTS, and LLM services for full functionality. For detailed setup instructions, refer to the [Agents Service README](https://github.com/fireredchat-submodules/agents/blob/fireredchat/README.md).

## Acknowledgements

We extend our gratitude to the following open-source projects:
- [livekit/livekit (RTC)](https://github.com/livekit/livekit)
- [livekit/agents](https://github.com/livekit/agents)
- [livekit/agents-playground](https://github.com/livekit/agents-playground)
- [speechbrain/spkrec-ecapa-voxceleb](https://huggingface.co/speechbrain/spkrec-ecapa-voxceleb)
- [google-bert/bert-base-multilingual-cased](https://huggingface.co/google-bert/bert-base-multilingual-cased)

## Disclaimer
The content provided is for academic purposes only and is intended to demonstrate technical capabilities.
