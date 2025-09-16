# FireRedTTS Accelerated Service

Accelerated FireRedTTS1 with RedServing

## Setup

1. Download the FireRedTTS model:
   ```bash
   # Note! this weight is for non-commercial usage only
   huggingface-cli download --resume-download --repo-type model FireRedTeam/FireRedTTS-1S --revision fireredtts1s_4_chat --local-dir ./tts_4_chat
   ```

2. Run service:
   ```bash
   docker run -td --name ttsserver --security-opt seccomp:unconfined -v "$(pwd)/tts_4_chat/pretrained_models:/workspace/models/redtts" -p 8081:8081 fireredchat/fireredtts1-server:latest bash /workspace/run.sh --llm --svc_config_path /workspace/svc.yaml --port 8081 --http_uri=/v1/audio/speech
   ```

## API Usage

**Endpoint**: `POST /v1/audio/speech`

**Example using curl**:
```bash
curl https://localhost:8081/v1/audio/speech \
    -H "Content-Type: application/json" \
    -d '{
    "input": "哈喽，你好呀～",
    "voice": "f531",
    "response_format": "mp3"
    }' --output audio.mp3
```

**Response**:
```bash
<audio binary>
```

## Notice
This TTS model weight is not under Apache2.0 (FireRedTeam/FireRedTTS-1S, revision fireredtts1s_4_chat) and the voice f531 included are for non-commercial usage only. You should also not use any f531 derivatives to train/distill your own model. 