# AI Photo & Video Generator

**Brand:** AI Studio Lucknow  
**Project Owner:** `aistudio.lucknow@gmail.com`

A complete, mobile-first, bilingual (**English & Hindi**) AI Photo & Video Generator built with React, TypeScript, Tailwind CSS, Node.js, Express, optional Firebase Auth/Firestore, Gemini API, and fal.ai **MiniMax H3 Max Turbo**.

---

## Features

- **Text to Photo**: 9 visual styles (`Photorealistic`, `Cinematic`, `Anime`, `3D Render`, `Indian Heritage`, `Fantasy`, `Cyberpunk`, `Vintage Film`, `Digital Art`), aspect ratios, quality levels, prompt enhancement, download, Save to Gallery, and regenerate.
- **Text to Video**: Uses the official fal.ai Queue REST API endpoint `minimax/h3-max-turbo/text-to-video` with **5s / 480p** defaults, supported durations (`5s`, `10s`, `15s`), supported resolutions (`480p`, `768p`, `1080p` — disabling unsupported `360p` and `720p`), `16:9` / `9:16` aspect ratios, live queue polling & logs, Play/Pause, MP4 download, and Save to Gallery.
- **Image to Video**: Upload PNG/JPG/WEBP (up to 10 MB), preview instantly in Free Mode, add motion prompts, and animate via `minimax/h3-max-turbo/image-to-video` (**5s / 480p** defaults) when configured.
- **Strict Cost Control ($3.00 USD Testing Cap)**:
  - Pre-request USD cost estimator & projected session total (tracked on both client and server)
  - Mandatory user confirmation modal before every paid AI request
  - Maximum duration cap (`5s`, `10s`, `15s`)
  - Single-flight generation lock (only 1 request at a time)
  - Zero automatic retries on paid API calls
  - *Note:* The `$3.00 USD` tracker is a software pre-flight guardrail/estimator, NOT a provider-enforced hard billing cap. Hard spending caps are only guaranteed when configured inside your fal.ai and Google Cloud billing dashboards.
- **Free Mode (No API Keys Required)**:
  - Free prompt writing, sample prompts, and local prompt enhancement
  - Local image/video upload, preview, and IndexedDB browser gallery
  - Clear `FREE / DEMO MODE` and `NOT CONFIGURED` indicators—never fabricates fake AI images or videos.
- **Bilingual UI**: Switch seamlessly between **English** and **हिन्दी (Hindi)**.

---

## Mobile-Friendly Local Setup

1. **Install Node.js 20+** and clone the repository:
   ```bash
   npm install
   ```
2. **Create `.env` from `.env.example`** (never commit `.env` or real keys):
   ```bash
   cp .env.example .env
   ```
   Add only the keys you want to activate on the backend:
   - `FAL_KEY` — Enables real `minimax/h3-max-turbo` video generation
   - `GEMINI_API_KEY` — Enables Gemini prompt enhancement and real image generation
   - `FIREBASE_PROJECT_ID` + `VITE_FIREBASE_*` — Optional Firebase Authentication & Firestore gallery sync
3. **Run the full-stack server (Express + Vite on port 3000)**:
   ```bash
   npm run dev
   ```
   Open `http://localhost:3000` on your computer or Android phone browser on the same Wi-Fi network (`http://<your-local-ip>:3000`).

---

## API Routes (Backend)

| Method | Endpoint | Purpose |
| :--- | :--- | :--- |
| `GET` | `/api/health` | Express server health check |
| `GET` | `/api/config/status` | Reports `CONFIGURED` vs `NOT_CONFIGURED` status and server spend estimate (never exposes keys) |
| `POST` | `/api/config/reset-budget` | Resets the server-side software session spend counter |
| `POST` | `/api/image/enhance` | Enhances prompts via Gemini `gemini-4-hydrogen` or Free Mode local enhancer |
| `POST` | `/api/image/generate` | Confirmed server-side Gemini image generation |
| `POST` | `/api/video/generate` | Confirmed `minimax/h3-max-turbo` text-to-video or image-to-video submission |
| `GET` | `/api/video/status/:id` | Polls fal.ai `status_url` and retrieves final MP4 from `response_url` |
| `POST` | `/api/video/cancel/:id` | Cancels polling / requests remote cancellation |

---

## GitHub Pages Deployment (Frontend Only)

This repository includes `.github/workflows/deploy.yml` configured with Vite base path `/ai-video-generator/`.

### Important Architecture Note
- **GitHub Pages hosts static frontend files (`dist/`) ONLY.**
- **GitHub Pages CANNOT run the Node.js/Express backend (`server/server.ts`)** and cannot keep `FAL_KEY` or `GEMINI_API_KEY` secret.
- On GitHub Pages without a separate backend, the app runs safely in **Free Mode** (prompt writing, local enhancement, local gallery, image upload preview).
- To enable real paid AI generation with a GitHub Pages frontend, deploy `server/server.ts` on a Node.js host (Render, Railway, Google Cloud Run, or a VPS), set `ALLOWED_ORIGIN` on the backend, and set `VITE_API_BASE_URL` when building the frontend.

### Steps to Deploy on GitHub Pages (`aistudiolucknow-png/ai-video-generator`)
1. Push your code to `https://github.com/aistudiolucknow-png/ai-video-generator` on branch `main`:
   ```bash
   git add .
   git commit -m "Deploy AI Photo & Video Generator"
   git push origin main
   ```
2. In GitHub **Settings → Pages → Build and deployment**, select **GitHub Actions**.
3. The included `.github/workflows/deploy.yml` will automatically run `GITHUB_PAGES=true npm run build` (`base: '/ai-video-generator/'`) and deploy to:
   `https://aistudiolucknow-png.github.io/ai-video-generator/`
4. You can also test the GitHub Pages build locally:
   ```bash
   npm run build:gh-pages
   ```
