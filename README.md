# Job Aggregator — Fullstack Platform with AI Scoring

> End-of-year fullstack project: a job & internship aggregator with a Python AI microservice that scores offers by semantic similarity to a search query.

[![Next.js](https://img.shields.io/badge/Next.js-16-000000?style=flat&logo=nextdotjs&logoColor=white)](https://nextjs.org/)
[![Express](https://img.shields.io/badge/Express-TypeScript-000000?style=flat&logo=express&logoColor=white)](https://expressjs.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-IA%20service-009688?style=flat&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![Jest](https://img.shields.io/badge/Tests-Jest%20%2B%20Supertest-C21325?style=flat&logo=jest&logoColor=white)](https://jestjs.io/)

## What it is

Job Aggregator is the **end-of-year project** of my first year at Epitech: a complete fullstack platform that collects, normalizes, and surfaces job & internship offers, with an extra twist — a **Python AI microservice** that ranks offers by semantic similarity to a user's free-text query (e.g. *"Stage Python Paris backend"*) using sentence embeddings.

The whole stack runs in **Docker Compose**: frontend, backend API, AI service, PostgreSQL database, and an Adminer UI for DB inspection.

## Stack

### Frontend
| Tool          | Purpose                              |
| ------------- | ------------------------------------ |
| Next.js 16    | React framework with App Router      |
| TypeScript    | Static typing across the UI          |
| Tailwind CSS  | Utility-first styling                |

### Backend API
| Tool                  | Purpose                                  |
| --------------------- | ---------------------------------------- |
| Node.js + Express 5   | REST API server                          |
| TypeScript            | Strict typing on routes, controllers, DB |
| PostgreSQL 15         | Relational database                      |
| Zod                   | Runtime payload validation               |
| JWT + bcrypt          | Auth + password hashing                  |
| Nodemailer            | Transactional emails                     |
| express-rate-limit    | Per-route throttling                     |
| Jest + Supertest      | Unit + API tests                         |

### AI Service
| Tool                          | Purpose                                                  |
| ----------------------------- | -------------------------------------------------------- |
| FastAPI                       | Lightweight Python service                               |
| `sentence-transformers`       | `all-MiniLM-L6-v2` embeddings (80 MB model, loaded once) |
| `psycopg2`                    | Direct PostgreSQL access for offer retrieval             |
| NumPy                         | Cosine similarity ranking                                |

### DevOps
| Tool            | Purpose                                  |
| --------------- | ---------------------------------------- |
| Docker Compose  | One-command orchestration of 5 services  |
| Adminer         | Web UI for DB inspection (port 8282)     |

## Architecture

```
┌─────────────────┐         ┌───────────────────┐         ┌──────────────┐
│  Frontend       │         │  Backend API      │         │  PostgreSQL  │
│  Next.js :3000  │ ──HTTP─▶│  Express :5001    │ ──SQL──▶│  :5432       │
└─────────────────┘         │  (TS, Zod, JWT)   │         └──────┬───────┘
                            └──────┬────────────┘                │
                                   │                             │
                                   │ /score                      │
                                   ▼                             │
                            ┌───────────────────┐                │
                            │  AI microservice  │ ─direct SQL────┘
                            │  FastAPI :8000    │
                            │  MiniLM embeddings│
                            └───────────────────┘
```

- The **AI service** boots, loads the embedding model once (`all-MiniLM-L6-v2`), and exposes a `/score` endpoint
- Given a free-text query, it embeds the query, fetches offers from PostgreSQL, computes cosine similarity, and returns the top N matches with scores
- The backend uses standard CRUD endpoints for accounts, saved offers, admin, etc.

## Features

- **Account system** — register, login, JWT sessions, secured routes, rate-limited endpoints
- **Offer aggregation & normalization** — heterogeneous sources normalized into a single schema
- **Search & filtering** — by company, location, contract type, keywords
- **AI semantic search** — full-text query → embedding → top-ranked offers via cosine similarity
- **Saved offers** — per-user shortlist with one-click apply modal
- **Admin panel** — for offer & user management
- **Dashboard** — usage stats, charts (Recharts/Chart-like component)
- **Transactional emails** — Nodemailer for confirmations
- **End-to-end Dockerized** — one `docker compose up`

## Documentation

Beyond the code, the project ships with a full set of **Architecture Decision Records** in `docs/ADR/` :

- **Architecture** — service split, communication patterns, scaling rationale
- **Data** — schema design, IA service design, feature data analytics
- **Security** — authentication choices, password policy, rate limiting
- **CI** — pipeline design
- **Deployment** — container strategy and environment variables
- **Accessibility** — WCAG audit results with screenshots (dashboard, login, offer detail, offers list)

Plus a **sourced market scan** and a **value proposition** document, because shipping isn't only about the code.

## What I learned

- **Designing a multi-service architecture** — splitting a monolithic instinct into 3 independent services (web, API, AI) talking over HTTP and SQL, each containerized with healthchecks and explicit dependencies
- **Semantic search without a vector DB** — applying `sentence-transformers` directly on a PostgreSQL-backed dataset, embedding queries at request time, ranking with NumPy cosine similarity
- **TypeScript in production** — strict types on Express routes, request validation with Zod schemas, types shared mentally between API contracts and frontend consumers
- **Test discipline** — unit tests for the offer normalizer, API tests with Supertest, real coverage on the critical paths
- **Documentation as a deliverable** — writing ADRs the way real teams do, defending each architectural choice in a 5-paragraph note, instead of just shipping a README

## Tested at

- Defense + Review with my pedagogical lead in May 2026
- Final mark on the project follow-up : **90/100** (progressive improvement from 20 → 50 → 90 over three review milestones)

## Project context

- **Year** : 2025–2026
- **School** : Epitech Bachelor, Mulhouse
- **Module** : B-YEP-200 — Year-End Project
- **Team** : 4 members

## Note

This is a portfolio summary. The full source code is private per Epitech academic policy. Available on request for recruiters.

---

[Imran Nogueira](https://github.com/imrannnn30)
