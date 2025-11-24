# Fitness Life Mantra – Local AI Assistant (Neocortex-based)

## 0. Goal

Spin up a **local-first AI assistant** using the [Neocortex](https://github.com/ankityadavv2014/neocortex) codebase, with:

* Local LLMs via **Ollama** (Llama, Phi, Qwen, Mistral, etc.).
* A web UI (Next.js) for chat and agents.
* A custom **"Fitness Life Mantra"** agent we can extend later.
* A foundation we can integrate into any website / app via HTTP APIs or embeddable UI (planned in next phase).

This document focuses on **getting it running immediately** on a dev machine. Customization and integrations are "Phase 2".

---

## 1. Prerequisites

Install these first:

1. **Git**
2. **Node.js 20+** (LTS recommended)

   * Check: `node -v`
3. **pnpm** (package manager Neocortex uses)

   ```bash
   npm install -g pnpm
   ```
4. **Docker** (for Postgres DB via Docker)

   * Docker Desktop or system Docker engine.
5. **Ollama** (for local LLMs) – [https://ollama.com](https://ollama.com)

   After installing Ollama, ensure it runs:

   ```bash
   ollama --version
   ```

---

## 2. Set up Ollama + Local LLMs

We'll use Ollama as the unified local LLM engine. Neocortex already supports an `OLLAMA_BASE_URL` env var.

### 2.1 Start Ollama

Ollama usually starts its daemon automatically. If needed:

```bash
ollama serve
```

By default it exposes an HTTP API at:

```text
http://localhost:11434
```

Neocortex expects:

```text
http://localhost:11434/api
```

### 2.2 Pull some local models

You can pull **multiple open-source models**; Neocortex will talk to them via Ollama:

```bash
# Good general models
ollama pull llama3.1
ollama pull phi3
ollama pull mistral
ollama pull qwen2.5

# Smaller / faster options for low-resource machines
ollama pull phi3:mini
ollama pull llama3.2:3b
```

You can always add more later. The key is that **Ollama knows the model name**, e.g. `llama3.1`, `phi3`, etc.

---

## 3. Clone Neocortex (Base Code)

```bash
git clone https://github.com/ankityadavv2014/neocortex.git
cd neocortex
```

Install dependencies (this also auto-generates a `.env` file at the project root).

```bash
pnpm install
```

---

## 4. Configure Environment (.env)

The install step creates `.env`. Open it in VS Code and set **minimal values** to run locally with Ollama.

Example **minimal .env** (safe starter template):

```env
# === LLM Providers ===
# Keep cloud keys empty if we want purely local for now.
GOOGLE_GENERATIVE_AI_API_KEY=
OPENAI_API_KEY=
XAI_API_KEY=
ANTHROPIC_API_KEY=
OPENROUTER_API_KEY=

# Local LLM via Ollama
OLLAMA_BASE_URL=http://localhost:11434/api

# === Auth ===
# Generate a random string here (can be any long random value)
BETTER_AUTH_SECRET=change_this_to_a_random_string
BETTER_AUTH_URL=http://localhost:3000

# === Database ===
# We'll run Postgres locally via Docker below.
POSTGRES_URL=postgres://neo_user:neo_password@localhost:5432/neocortex_db

# === Tools (optional) ===
EXA_API_KEY=
FILE_BASED_MCP_CONFIG=false

# === File Storage (default – Vercel Blob, but it's fine locally even if token is empty) ===
FILE_STORAGE_TYPE=vercel-blob
FILE_STORAGE_PREFIX=uploads
BLOB_READ_WRITE_TOKEN=

# === OAuth (optional; can stay empty for now) ===
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
GOOGLE_FORCE_ACCOUNT_SELECTION=
MICROSOFT_CLIENT_ID=
MICROSOFT_CLIENT_SECRET=
MICROSOFT_TENANT_ID=
MICROSOFT_FORCE_ACCOUNT_SELECTION=

# === App Access Controls ===
DISABLE_SIGN_UP=
NOT_ALLOW_ADD_MCP_SERVERS=
```

> You can adjust `neo_user`, `neo_password`, and `neocortex_db` to whatever you like, just keep it consistent with the Postgres step.

---

## 5. Start Postgres with Docker

Neocortex includes a convenience script to start Postgres via Docker.

From the project root:

```bash
# Start Postgres in Docker
pnpm docker:pg
```

This uses `docker/compose.yml` to bring up a Postgres container. Make sure `POSTGRES_URL` in `.env` matches the credentials set in that compose file (if they differ, edit either `.env` or `docker/compose.yml` to match).

Then run database migrations:

```bash
pnpm db:migrate
```

---

## 6. Run the App Locally

### Option A (recommended for dev): Hot-reload dev server

```bash
pnpm dev
```

This runs the Next.js dev server. Open:

```text
http://localhost:3000
```

in your browser.

### Option B: "Production-style" local run

```bash
pnpm build:local && pnpm start
```

Again, access via `http://localhost:3000`.

Neocortex docs confirm `http://localhost:3000` as the default entry point.

---

## 7. First-time Setup in the UI

1. Open `http://localhost:3000`.
2. Sign up / log in (local auth via Better Auth).
3. Go to whatever **Settings / Providers / Models** menu exists (exact label may differ slightly, but there will be a place to pick model providers).
4. Ensure **Ollama** is recognized:

   * It will use `OLLAMA_BASE_URL` from `.env`.
   * You should be able to select models by name, e.g. `llama3.1`, `phi3`, `mistral`, `qwen2.5`, etc.

If you see any provider errors, double-check:

* `OLLAMA_BASE_URL=http://localhost:11434/api`
* `ollama serve` is running.
* You actually pulled the model (`ollama list` to confirm).

---

## 8. Create the "Fitness Life Mantra" Agent

Neocortex supports **custom agents** with their own system prompts and tools.

In the UI (once logged in):

1. Look for "Agents" / "Custom Agents" / similar section.

2. Create a new agent, e.g.:

   * **Name:** `Fitness Life Mantra`
   * **Slug / handle (if present):** `@flm_coach`
   * **Default Model:** your favorite local model (e.g. `llama3.1` or `phi3`).

3. For the **System Prompt**, paste something like:

   > You are the "Fitness Life Mantra" (FLM) personal coach.
   > Your job is to help the user live life to the fullest, staying fit **physically, mentally, and emotionally**, no matter what is happening around them.
   >
   > * Speak in a motivating, practical, and grounded tone.
   > * Blend science-backed advice with creativity, movement, dance, breathwork, and daily rituals.
   > * Help the user craft daily mantras, short workout / movement ideas, and emotional reset routines.
   > * When the user asks about content creation, help them turn FLM principles into scripts, captions, and storyboards.
   > * When unsure, ask concise clarifying questions instead of guessing.

4. Save the agent.

5. In the chat UI, invoke it using its name or handle (e.g. select `Fitness Life Mantra` or type `@flm_coach` if that's supported).

That gives you an **FLM-branded local LLM assistant** running on your machine.

---

## 9. Using Multiple Local LLMs

Because everything goes through **Ollama**, using "all local LLMs" basically means:

1. Pull as many models as you want with `ollama pull`.
2. Expose them via the same `OLLAMA_BASE_URL`.
3. In Neocortex:

   * Create different **agents** bound to different models (e.g. one for fast Q&A, one for deep reasoning).
   * Or switch models per-chat if the UI allows.

Examples:

* `FLM-Fast` → `phi3:mini`
* `FLM-Deep` → `llama3.1`
* `FLM-Creative` → `mistral` or `qwen2.5`

---

## 10. High-level Integration Plan (Phase 2)

Once the app is running reliably, we can integrate it with **any website or application** in (at least) these ways:

### 10.1 Embed the UI (quick & dirty)

* Host Neocortex somewhere (or keep on local network).
* Embed `http://<host>:3000` or a custom chat page in:

  * An `<iframe>` inside any site.
  * A WebView inside mobile apps or desktop shells (Electron, Tauri, etc.).

### 10.2 Expose a minimal HTTP API (recommended)

Inside this repo (or a small companion service), we can add an endpoint such as:

* `POST /api/flm-assistant`
* Body: `{ "messages": [...], "agent": "flm_coach" }`
* Response: `{ "reply": "...", "model": "llama3.1", ... }`

The internal implementation would:

* Receive messages from any website/app.
* Call Neocortex's chat logic (or directly call Ollama for certain simple flows).
* Return a simple JSON payload for the frontend to render.

**Frontend websites/apps** (Next.js, React SPA, mobile app, etc.) can then:

```ts
const res = await fetch("/api/flm-assistant", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ messages, agent: "flm_coach" }),
});
const data = await res.json();
```

> Implementation details depend on how Neocortex structures its internal chat APIs; we'll wire this up once we inspect the `src/app/api` routes and the chat service modules.

### 10.3 MCP / Tools Integration

Neocortex already supports **MCP (Model Context Protocol)** and custom tools.

Later we can:

* Expose FLM data (user routines, playlists, workout templates, calendar events, etc.) as MCP tools.
* Let the FLM agent call these tools to read/write information into other apps.

---

## 11. Quick "Do This Now" Checklist

If you just want it **running right now**:

1. Install Node 20+, pnpm, Docker, Ollama.
2. `git clone https://github.com/ankityadavv2014/neocortex.git`
3. `cd neocortex`
4. `pnpm install`
5. Edit `.env`:

   * Set `OLLAMA_BASE_URL=http://localhost:11434/api`
   * Set `BETTER_AUTH_SECRET=<random>`
   * Set `POSTGRES_URL=postgres://neo_user:neo_password@localhost:5432/neocortex_db`
6. `pnpm docker:pg`
7. `pnpm db:migrate`
8. `pnpm dev`
9. Open `http://localhost:3000`
10. Create the **Fitness Life Mantra** agent with the system prompt above and choose a local model (e.g. `llama3.1`).

After that, you have a **local FLM personal assistant** powered by open models, ready for later customization and integration into all your FLM properties.
