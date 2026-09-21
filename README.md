# whisper-stt

Central Whisper STT for all Hermes agents on the VPS — deployed via **Dokploy**
(compose service `whisper-stt`).

- Endpoint: `http://127.0.0.1:8000/v1/audio/transcriptions` (loopback only)
- Health: `curl http://127.0.0.1:8000/health` → `OK`
- Model cache is an **external volume** (`whisper-stt-ic4jw4_whisper-models`)
  so the model never re-downloads across redeploys.

No shared Docker networks on purpose — consumers connect via the loopback
port mapping. Admin docs: `docs/HERMES-SETUP-GUIDE.md` on the VPS.
