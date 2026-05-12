# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Vue 3 + Vite microfrontend for the smmall-uploader Cloudflare Worker. Provides Upload and History views with a cyberpunk dark theme.

## Commands

```bash
npm install
npm run dev       # Vite dev server (localhost:5173)
npm run build     # Production build → dist/
npm run preview   # Preview production build
```

## Architecture

- `src/main.js` — entry; mounts App to `#app`
- `src/App.vue` — tab nav (Upload / History) via `view` ref
- `src/components/UploadForm.vue` — posts `multipart/form-data` to worker `/upload`
- `src/components/UploadHistory.vue` — fetches from worker `/history` on mount
- `src/style.css` — global cyberpunk dark theme (monospace, CSS custom properties)

Worker base URL is hardcoded in both components: `https://smmall-uploader.cybersoulja.workers.dev`

## Backend

Worker lives at `../smmall-uploader` (repo: `cybersoulja/smmall-uploader`). Endpoints:

- `POST /upload` — accepts `file`, `link` (optional), `note` (optional)
- `GET /history` — returns JSON array of upload records, newest first
