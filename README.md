## Hey 👋

I'm Sinse — solo dev building **AI-powered tools** on self-hosted infrastructure.

Transition Admin Infra Sécurisée (PTP, déc 2026). Building in the open as I go.

### 🌐 petit-pont.com — multi-app AI ecosystem

Self-hosted on Proxmox + Cloudflare Zero Trust. 5 apps live, isolated per-domain with email policies, all running on a shared LiteLLM proxy + Dify orchestration.

| Domain | App | Stack | Status |
|---|---|---|---|
| `academia.petit-pont.com` | **AcademIA** — adaptive language learning (EN/ES, soon IT/DE/JP/RU) | SvelteKit + FastAPI + Dify + Postgres | Live |
| `marie.petit-pont.com` | **Maître Comptable** — Q&A omniscient compta + RAG 22 PDFs (Qdrant) | SvelteKit + FastAPI + Dify advanced-chat | Live |
| `coach.petit-pont.com` | **Coach Sportif** — V0.1 Q&A muscu, V0.2+ tracking + programmes + nutrition | SvelteKit + FastAPI + Dify chat | Live (V0.1) |
| `sinse.petit-pont.com` | **Self-learn** placeholder | TBD | Reserved |
| `petit-pont.com` (apex) | Public landing | Static | Live |

### 🚀 Flagship — [AcademIA](https://github.com/Sinsemilila/academIA)

AI language learning platform — adaptive lessons powered by 5+ LLMs (Groq, Mistral, OpenAI, Gemini via LiteLLM proxy). Multi-agent Dify chatflows (Teacher EN/ES + Maestro), real-time error detection w/ deterministic Lyster-style rules engine, oracle harness multi-judge cross-provider (κ Opus 0.85+), behavior-adaptive pedagogy, CECRL exams (Cambridge style + DELE/CILS/JLPT/TORFL roadmap Wave 2-4), gamification.

**~70 sessions of solo dev, ~170 architectural locks, ADRs all the way down.** SvelteKit + FastAPI + n8n + PostgreSQL (PgBouncer transaction-mode) + Qdrant.

### 🛠 Tech stack

![SvelteKit](https://img.shields.io/badge/-SvelteKit_5-FF3E00?logo=svelte&logoColor=white&style=flat-square)
![Tailwind](https://img.shields.io/badge/-Tailwind_v4-06B6D4?logo=tailwindcss&logoColor=white&style=flat-square)
![FastAPI](https://img.shields.io/badge/-FastAPI-009688?logo=fastapi&logoColor=white&style=flat-square)
![Python](https://img.shields.io/badge/-Python_3.13-3776AB?logo=python&logoColor=white&style=flat-square)
![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?logo=typescript&logoColor=white&style=flat-square)
![Docker](https://img.shields.io/badge/-Docker-2496ED?logo=docker&logoColor=white&style=flat-square)
![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-4169E1?logo=postgresql&logoColor=white&style=flat-square)
![Qdrant](https://img.shields.io/badge/-Qdrant-DC382D?logoColor=white&style=flat-square)
![Dify](https://img.shields.io/badge/-Dify-1C64F2?logoColor=white&style=flat-square)
![n8n](https://img.shields.io/badge/-n8n-EA4B71?logo=n8n&logoColor=white&style=flat-square)
![LiteLLM](https://img.shields.io/badge/-LiteLLM-4B0082?style=flat-square)
![Cloudflare](https://img.shields.io/badge/-Cloudflare_Zero_Trust-F38020?logo=cloudflare&logoColor=white&style=flat-square)
![Proxmox](https://img.shields.io/badge/-Proxmox-E57000?logo=proxmox&logoColor=white&style=flat-square)

### 🤖 How I work

I build with **Claude Code (Opus 4.7, 1M context)** — heavy custom workflow toolkit :

- `ship` (granular commits + auto-push + secret pre-check)
- `safepoint` (rollback checkpoints), `decision` (atomic ADRs)
- `/handoff` (auto-detect projets touchés, vault writes per-app)
- `/project <app>` (deep load context per-app one-shot)
- Custom subagent `vault-reader` (Haiku 4.5) for cross-project knowledge retrieval (~74% token savings vs raw vault load)
- Pedagogy reviewer + Dify patcher subagents

Vault Obsidian + Syncthing 3 devices for personal knowledge graph.

### 📦 Apps & repos

| App | Repos | Domain |
|---|---|---|
| **AcademIA** | [academIA](https://github.com/Sinsemilila/academIA) (public) | academia.petit-pont.com |
| **Marie** (Maître Comptable) | marie + marie-api (private) | marie.petit-pont.com |
| **Coach** (Coach Sportif) | coach + coach-api (private) | coach.petit-pont.com |
| **Eisenday** | [Eisenday-app](https://github.com/Sinsemilila/Eisenday-app) (private) | mobile-first SvelteKit + Capacitor + i18n 13 langues |
| **petit-pont-infra** | private — shared infra (CF + nginx + sops bundle) | cross-app |
| **petit-pont-ui** | private — shared UI components Svelte | cross-app |

### 📍 Contact

`sinseproduction@gmail.com` · Montpellier, France 🇫🇷
