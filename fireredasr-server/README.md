# FireRedASR Microservice

A FastAPI-based microservice for speech-to-text transcription using FireRedASR.

## Setup

1. Download the FireRedASR model:
   ```bash
   cd server
   mkdir -p models
   # Download FireRedASR-AED-L & PUNC-BERT model and place it in models/
   git clone https://huggingface.co/FireRedTeam/FireRedChat-punc models/PUNC-BERT
   pushd models/PUNC-BERT && git lfs pull && popd
   git clone https://huggingface.co/FireRedTeam/FireRedASR-AED-L models/FireRedASR-AED-L
   pushd models/FireRedASR-AED-L && git lfs pull && popd
   ```

2. Build the Docker image:
   ```bash
   docker build -t fireredasr-service .
   ```

3. Run the container:
   ```bash
   docker run -d \
     -p 8000:8000 \
     -v $(pwd)/models:/app/models \
     fireredasr-service
   ```

## API Usage

### Transcribe Audio

**Endpoint**: `POST /audio/transcriptions`

**Parameters**:
- `file`: Audio file (multipart/form-data)
- `media_type`: Response format (application/json or text/plain)

**Example using curl**:
```bash
curl -X POST \
  http://localhost:8000/audio/transciptions \
  -H "Content-Type: multipart/form-data" \
  -F "file=@audio.wav" \
  -F "media_type=application/json"
```

**Response**:
```json
{
  "sentences": [{
    "confidence": 0.8,
    "text": "transcribed text here"
    }],
  "wav_file": "audio.wav"
}
```

## Environment Variables

- `FIREREDASR_PATH`: Path to FireRedASR installation (default: /app/fireredasr)
- `MODEL_DIR`: Path to model directory (default: /app/models) 