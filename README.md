# Kelvid Backend 🎬

FastAPI API for text-to-video generation via Hugging Face Inference Providers.
Pure API — no frontend bundled. Pair with [kelvid-frontend](../kelvid-frontend).

## Quickstart

```powershell
copy .env.example .env
# edit .env → paste your HF_TOKEN (https://huggingface.co/settings/tokens)
python -m pip install -r requirements.txt
python -m uvicorn main:app --host 127.0.0.1 --port 8000
```

API docs: http://localhost:8000/docs

## Docker

```powershell
docker compose up --build
```

## Endpoints

| Method | Path | Notes |
|---|---|---|
| GET | `/api/health` | status, version, active jobs |
| GET | `/api/models` | recommended models |
| POST | `/api/generate` | queue job → `{job_id}` |
| GET | `/api/jobs/{job_id}` | poll `queued\|running\|done\|failed` |
| GET | `/api/jobs` | recent jobs |
| POST | `/api/jobs/{job_id}/retry` | retry failed (API key if set) |
| GET | `/api/gallery` | persistent gallery |
| DELETE | `/api/gallery/{filename}` | delete video (API key if set) |

Videos served at `/videos/{filename}`.

## Env (`backend/.env`)

```
HF_TOKEN=hf_...
HF_PROVIDER=fal-ai
HF_MODEL=Wan-AI/Wan2.2-TI2V-5B
MAX_CONCURRENT_JOBS=2
MAX_QUEUE=20
RATE_LIMIT_PER_MIN=20
CORS_ORIGINS=*            # set to your frontend URL in prod, e.g. https://kelvid.vercel.app
KELVID_API_KEY=           # optional; if set, clients send X-API-Key
PORT=8000
```

## Notes

- Jobs + gallery persist in `outputs/` (`jobs.json`, `gallery.json`).
- Never commit `.env`.
