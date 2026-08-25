# Elite NFL Intelligence

Multi-source NFL sports-intelligence and probabilistic analytics platform.

This public repository is a **safe showcase snapshot** of the engineering architecture. It describes the system for portfolio and hiring review. Proprietary prediction weights, production secrets, owner license keys, and private user data are **not** included.

> Simulated betting only. The platform does **not** place real-money wagers.

---

## Project Overview

Elite NFL Intelligence combines independent free/public NFL data sources into a normalized internal database, then exposes live boards, injury intelligence, weather context, and (future) calibrated probability outputs.

**Credits**
- Lead Developer: **M. S**
- Lead Architect: **J. V**

---

## Key Features

- NFL Weekly Board with verified live/historical scoreboard data
- Live Injury Center with status change tracking
- Multi-source provider adapters (replaceable interfaces)
- Source fusion / consensus metadata
- Stadium weather for outdoor venues (indoor games skipped)
- nflverse historical schedules + play-by-play foundation (recent 3 seasons)
- Derived team / player metrics (EPA, success rate windows)
- Opponent-adjusted ranking architecture
- Activation / licensing gate (server-side validation)
- Simulated $5 bankroll / friends leaderboard architecture (application surface)
- Settings / About panel

---

## Live Data Pipeline

```
Free providers (ESPN, NWS, nflverse, optional Odds API free tier)
        ↓
Provider adapters (interfaces: SportsData / Odds / Injury / Weather / Historical / News / Roster)
        ↓
Normalization + source records (provider, timestamps, freshness, confidence, canonical IDs)
        ↓
Internal SQLite/PostgreSQL-ready database
        ↓
FastAPI endpoints
        ↓
React client (activation-gated)
```

Polling is interval-based with caching. Time-sensitive feeds refresh faster than historical backfills. Stale sources are marked **STALE** rather than silently treated as live.

---

## Multi-Source Data Fusion

Overlapping facts retain provider provenance. Entity resolution maps name variants to one internal player/team/game ID. Conflicts are recorded instead of silently overwritten.

---

## NFL Matchup Engine / Game Lab

Game Lab is designed to consume opponent-adjusted rankings, weather context, injury consensus, and market lines. Detailed matchup UI expands as feature tables fill.

---

## Predictive Modeling

The production prediction engine is **server-side** and intentionally not shipped as open model weights in this showcase.

Modeling philosophy:
- Primary window: **most recent 3 NFL seasons**
- Older seasons: optional low-weight priors for rare conditions only
- Context continuity score architecture for QB / coach / scheme continuity
- Features must respect a **cutoff timestamp** (no future leakage)

Self-learning / calibration add-ons are separate and not part of this snapshot.

---

## Model Calibration

Walk-forward evaluation and calibration reporting are planned for Model Lab. This showcase does **not** advertise unverified accuracy percentages.

---

## Injury Intelligence

Live injury ingestion tracks status transitions (e.g. QUESTIONABLE → OUT) and emits internal events for later recalculation. Practice-report depth depends on free-source availability.

---

## Game Lab / Player Props / Simulated Betting

- Game Lab: deep matchup panels (phased)
- Player Props: model distributions only after trained projections exist
- Simulated betting: fake-money environment + friends leaderboard concepts — **no real wagering**

---

## Security / Licensing

- Critical logic stays server-side
- Activation uses username + activation key; backend redeems hashed keys
- STANDARD licenses expire 30 days after first redemption (server time)
- ADMIN_UNLIMITED licenses do **not** grant owner/admin application roles by default
- Secrets never live in frontend JavaScript
- Owner plaintext keys live outside Git in a private folder

See [SECURITY.md](SECURITY.md) for the public summary.

---

## Tech Stack

| Layer | Stack |
|-------|--------|
| Frontend | React, TypeScript, Vite, Tailwind |
| Backend | Python, FastAPI, SQLAlchemy, APScheduler |
| Data | SQLite (dev), PostgreSQL-ready, Redis-ready |
| Sources | ESPN public APIs, NWS, nflverse, optional The Odds API free tier |

---

## Architecture

```
frontend/          Activation gate + boards + Injury Center + Settings/About
backend/app/
  providers/       Replaceable adapters
  services/        Ingest, fusion, PBP metrics, licensing
  api/             Public JSON API + auth
  models/          Normalized entities + license tables
  jobs/            Background polling
docs/              Security notes
showcase/          Public-safe materials (this README)
```

---

## Screenshots

The live product UI stays in the private workspace. This public repo is the architecture write-up for hiring review, without production secrets or private user data.

---

## Future Development

- Attach separate self-learning / calibration engine
- Second free injury / practice source if a legitimate feed is found
- Open-Meteo historical weather joins for outdoor venues
- Desktop OS credential store for remembered sessions
- Public demo without production license keys

---

## Setup (non-secret demo)

Private production keys are not published. Local developers with the private workspace:

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt
copy ..\.env.example .env
uvicorn app.main:app --reload --port 8000

cd ..\frontend
npm install
npm run dev
```

---

## License Notice

Showcase documentation is provided for portfolio demonstration. Application source in private development remains proprietary unless separately licensed by the owners.
