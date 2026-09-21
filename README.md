# whisper-kokoro

Local voice stack for all Hermes agents on the VPS — **STT (Whisper) + TTS
(Kokoro)** as one Dokploy compose project, two services.

## whisper-stt (Speech-to-Text)

- Endpoint: `http://127.0.0.1:8000/v1/audio/transcriptions` (loopback only)
- Health: `curl http://127.0.0.1:8000/health` → `OK`
- Model cache: **external volume** `whisper-stt-ic4jw4_whisper-models` — never
  re-downloads across redeploys.

## kokoro-tts (Text-to-Speech)

- Endpoint: OpenAI-compatible `http://127.0.0.1:8880/v1/audio/speech`
  (loopback only); also `http://127.0.0.1:8880/web` for a test UI
- Health: `curl http://127.0.0.1:8880/health`
- Voices: `af_heart`, `af_bella`, `am_michael`, `bm_george`, …
- Model (~350 MB ONNX) downloads into the `kokoro-models` volume on first
  start; subsequent starts are fast.

## Hermes agent wiring

```yaml
tts:
  provider: openai          # OpenAI-compatible → kokoro
  openai:
    base_url: http://127.0.0.1:8880/v1
    model: kokoro
    voice: af_heart
    api_key: not-needed     # server ignores it, client needs it non-empty
```

## Notes

- No shared Docker networks on purpose — consumers connect via loopback
  port mappings. `docker compose up -d` only recreates changed services, so
  TTS redeploys don't bounce STT.
- Deployed via Dokploy (dashboard-visible). Admin docs on the VPS:
  `docs/HERMES-SETUP-GUIDE.md`.
