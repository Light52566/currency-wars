# Project Status

> Last updated: 2026-03-17
> Current phase: **Pre-development — Project scaffolding complete**

---

## Where We Left Off

Repository initialized and all foundational documents are in place. No application code exists yet. The next step is standing up the backend skeleton (FastAPI + DB) and the frontend skeleton (Next.js), then wiring them together with the first working competition flow.

---

## Current State

| Area | Status | Notes |
|---|---|---|
| Product spec | Done | `project_spec.md` — milestones, data model, API surface |
| Repo & git | Done | `github.com/Light52566/currency-wars` on `master` |
| Env config | Done | `.env.example` covers all adapters and services |
| CLAUDE.md | Done | Architecture, conventions, commands, test strategy |
| Changelog | Done | `CHANGELOG.md` — Keep a Changelog format |
| Backend skeleton | Not started | FastAPI app, models, DB, routers |
| Frontend skeleton | Not started | Next.js app, layout, basic pages |
| Order engine | Not started | Fill logic, fees, short selling |
| Data adapters | Not started | Online (yfinance/CoinGecko) + Offline (CSV) |
| Leaderboard SSE | Not started | Live score stream |
| UI: Dashboard | Not started | Charts, positions table, trade form |
| Tests | Not started | pytest (backend), Vitest (frontend) |
| CI/CD | Not started | GitHub Actions pipeline |

---

## Milestone Progress

```
[MVP  ]  ░░░░░░░░░░  0%   Core loop (create, join, trade, leaderboard)
[V1   ]  ░░░░░░░░░░  0%   Full asset classes, order types, spectator mode
[V2   ]  ░░░░░░░░░░  0%   Margin, replay, analytics
[Later]  ░░░░░░░░░░  0%   Scheduled comps, commentary, mobile polish
```

---

## Near-Future Tasks

### Agent: Backend Setup
Stand up the FastAPI skeleton with working DB and a health endpoint.

- [ ] Scaffold `backend/` directory structure (`main.py`, `config.py`, `database.py`)
- [ ] Define SQLAlchemy models: `Competition`, `Player`, `Order`, `Position`, `PriceSnapshot`
- [ ] Set up Alembic for migrations
- [ ] `GET /health` endpoint returns 200
- [ ] Wire `DATABASE_URL` from `.env`

### Agent: Frontend Setup
Stand up the Next.js skeleton with a home page and routing.

- [ ] Scaffold `frontend/` with Next.js App Router + TypeScript
- [ ] Decide and configure styling (Tailwind vs CSS Modules)
- [ ] Home page: "Create competition" form + "Join with code" input
- [ ] Dynamic route `app/[code]/page.tsx` renders competition room shell
- [ ] `lib/api.ts` typed fetch wrapper pointing at `NEXT_PUBLIC_API_URL`

### Agent: Competition API
Implement the competition lifecycle endpoints.

- [ ] `POST /competitions` — create, generate lobby code, return code
- [ ] `POST /competitions/{code}/join` — register named guest, return player token
- [ ] `POST /competitions/{code}/start` — transition state `lobby → active`
- [ ] `GET /competitions/{code}` — return competition details + current state
- [ ] `GET /competitions/{code}/leaderboard` — return ranked player list

### Agent: Order Engine
Core trading logic — the heart of the simulation.

- [ ] Market order fill at current snapshot price
- [ ] Fee deduction on fill
- [ ] Position open/close/update (VWAC)
- [ ] Short selling: negative quantity positions
- [ ] `POST /players/{id}/orders` and `GET /players/{id}/orders` endpoints
- [ ] Unit tests: fill price, fee math, short P&L

### Agent: Data Adapters
Pluggable price data layer.

- [ ] Define `DataAdapter` Protocol in `backend/data_adapters/base.py`
- [ ] `OfflineDataAdapter` — reads OHLCV from CSV files in `/data`
- [ ] Bundle sample offline data: AAPL, TSLA, BTC-USD, ETH-USD
- [ ] `OnlineDataAdapter` — yfinance for stocks; CoinGecko for crypto
- [ ] Price snapshot background task (runs every `PRICE_SNAPSHOT_INTERVAL_SECONDS`)

### Agent: CI/CD
GitHub Actions pipeline for automated quality checks.

- [ ] `ci.yml` — on PR: lint (ruff + eslint), type-check (tsc), run tests (pytest + vitest)
- [ ] `deploy.yml` — placeholder for future deployment step
- [ ] Add status badge to README

---

## Open Decisions

| Decision | Options | Priority |
|---|---|---|
| Default starting balance | $10,000 (suggested) | Before MVP |
| Styling approach | Tailwind vs CSS Modules | Before frontend setup |
| Lobby code expiry | Expire after N hours with no join, or never | Before MVP |
| Timezone handling | UTC everywhere (recommended) | Before MVP |
| Partial limit order fills | Support in MVP or defer to V1 | Before order engine |
