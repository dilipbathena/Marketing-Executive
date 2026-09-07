# AI Marketing Executive

A local-first MVP for creating marketing campaigns for a property, product, service, course, vehicle, or business. It includes a Next.js dashboard, FastAPI API, SQLite storage, media upload, an editable scene-by-scene video storyboard, and a local Ollama-compatible content provider.

House photos or videos are optional: upload them only if you want to use them in a campaign.

## What works in this MVP

- Create, view, and update campaigns.
- Capture marketing details: description, features, price, audience, location, and call-to-action.
- Upload images and videos to a campaign.
- Generate a draft marketing pack (ad copy, social posts, hashtags, and CTA). It works in template mode by default, or can call a local Ollama server.
- Create a storyboard with scenes and line-by-line editable overlay text.
- Preview the selected scene's uploaded media in the browser.
- An FFmpeg export endpoint is provided as a deliberate scaffold; it checks for FFmpeg and returns the export plan until rendering options are added.

## Requirements (Windows)

- Node.js 20 or newer: https://nodejs.org/
- Python 3.11 or newer: https://www.python.org/downloads/
- Optional: FFmpeg, added to `PATH`, for future video rendering: https://ffmpeg.org/download.html
- Optional: Ollama for local AI generation: https://ollama.com/

## Start the backend

Open PowerShell in this folder:

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m uvicorn app.main:app --reload --port 8000
```

The first start creates `backend/data/marketing.db` and `backend/uploads/` automatically.

### Easiest backend start

Right-click `start-backend.ps1` and choose **Run with PowerShell**, or run this from PowerShell in the project folder:

```powershell
.\start-backend.ps1
```

This creates an isolated Python environment, installs the backend requirements, and opens the API at http://localhost:8000. The interactive web dashboard still requires the frontend startup in the next section.

## Start the frontend

Open a second PowerShell window in this folder:

```powershell
cd frontend
npm install
npm run dev
```

Visit http://localhost:3000. The API documentation is at http://localhost:8000/docs.

## Local AI option (Ollama)

The app uses safe template generation unless you enable local AI. To use Ollama, install it, run a model such as `ollama pull llama3.2`, then set these backend environment variables before starting FastAPI:

```powershell
$env:AI_PROVIDER="ollama"
$env:OLLAMA_MODEL="llama3.2"
$env:OLLAMA_BASE_URL="http://localhost:11434"
python -m uvicorn app.main:app --reload --port 8000
```

No cloud API key is required. The provider interface lives in `backend/app/services/content_provider.py`, so another local or hosted provider can be added later.

## FFmpeg integration

`POST /api/campaigns/{id}/storyboard/export` detects FFmpeg and returns an export plan. It intentionally does not render video yet: production rendering needs choices for aspect ratio, transitions, fonts, audio, and file-storage strategy. The storyboard data already includes the timings and text needed for that next step.

## Project layout

```text
backend/   FastAPI, SQLite, uploads, AI-provider and FFmpeg scaffolds
frontend/  Next.js + Tailwind dashboard
```
