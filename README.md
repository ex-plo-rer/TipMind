<div align="center">

<img src="https://img.shields.io/badge/TipMind-Autonomous%20Fan%20Agent-6366f1?style=for-the-badge" alt="TipMind"/>

# TipMind
### Your Autonomous Fan Agent

**An AI agent that watches creators, feels the crowd, and tips in crypto — entirely on its own.**

*No buttons. No manual triggers. No human in the loop.*

<br/>

[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-009688?style=flat-square&logo=fastapi)](https://fastapi.tiangolo.com)
[![Next.js](https://img.shields.io/badge/Next.js-16-black?style=flat-square&logo=next.js)](https://nextjs.org)
[![xAI Grok](https://img.shields.io/badge/xAI-Grok--3--mini-white?style=flat-square)](https://x.ai)
[![WDK](https://img.shields.io/badge/Tether-WDK-26A17B?style=flat-square)](https://docs.wdk.tether.io)
[![Polygon](https://img.shields.io/badge/Polygon-Amoy-8247e5?style=flat-square&logo=polygon)](https://polygon.technology)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)

<br/>

**[🚀 Live Demo](https://frontend-lyart-six-24.vercel.app)** · **[📡 API](https://precious-spirit-production.up.railway.app/docs)** · **[🔗 On-Chain Txns](https://amoy.polygonscan.com/address/0xeED09Ed732aE030D58F36235FF445012992b9CDC)**

</div>

---

## The Problem

The creator economy runs on attention — but appreciation doesn't pay rent.

Fans want to support their favourite creators the moment something great happens: a debate win, a viral moment, a milestone crossed. But the moment passes. The wallet stays closed. The tip never happens.

Existing tip tools require intent, friction, and memory. Humans are bad at all three.

## The Solution

TipMind is a **fully autonomous AI agent** that monitors creator content in real time, interprets what's happening, and executes crypto tips the moment they're deserved — faster than any human, every time.

It watches YouTube channels continuously. It reads crowd emotion from live chat. It celebrates milestones with AI-crafted messages. And when a fan swarm triggers, it fires every participant's tip **simultaneously** — a coordinated on-chain burst impossible to do manually.

> **Set it up once. It runs forever. Creators earn. Fans contribute. Zero friction.**

---

## Screenshots

<div align="center">

![TipMind Dashboard](assets/Screenshot%202026-03-22%20at%2022.59.14.png)
*Live dashboard — real-time agent feed, active swarms, wallet balance*

![TipMind Transactions](assets/Screenshot%202026-03-22%20at%2022.59.30.png)
*Transaction log — on-chain tips with tx hashes, top creators leaderboard*

</div>

---

## How It Works

```
┌─────────────────────────────────────────────────────────────────────┐
│  YouTube RSS Feeds  (public — no API key required)                   │
│  MKBHD · t3.gg · Coin Bureau · Dave2D  + your own channels          │
└───────────────────────────┬─────────────────────────────────────────┘
                            │  polls every 90 s — zero human input
                            ▼
┌─────────────────────────────────────────────────────────────────────┐
│  Autonomous Poller  (backend/core/poller.py)                        │
│  • Detects new videos → injects WATCH_TIME + CHAT_MESSAGE events    │
│  • Fetches real engagement stats via YouTube Data API v3            │
│  • Recognises milestone keywords in titles via regex                │
└───────────────────────────┬─────────────────────────────────────────┘
                            │  asyncio pub/sub EventBus
         ┌──────────────────┼──────────────┬──────────────────┐
         ▼                  ▼              ▼                  ▼
  ┌─────────────┐  ┌─────────────┐  ┌───────────────┐  ┌───────────┐
  │  WatchTime  │  │  Emotion    │  │  Milestone    │  │  Swarm    │
  │  TipAgent   │  │  ChatAgent  │  │  TipAgent     │  │  Agent    │
  │             │  │             │  │               │  │           │
  │  ≥50% watch │  │  rolling    │  │  DEBATE_WIN   │  │ fan pool  │
  │  → Grok     │  │  sentiment  │  │  VIEWS_100K   │  │ parallel  │
  │  → micro-tip│  │  spike      │  │  SUBS_MILE..  │  │ release   │
  └──────┬──────┘  └──────┬──────┘  └───────┬───────┘  └─────┬─────┘
         └────────────────┴─────────────────┴────────────────┘
                                    │
                       ┌────────────▼────────────┐
                       │   xAI Grok-3-mini        │
                       │                          │
                       │  · Sizes each tip        │
                       │  · Reads crowd energy    │
                       │  · Writes announcements  │
                       │  · Returns JSON decisions│
                       └────────────┬─────────────┘
                                    │
                       ┌────────────▼────────────┐
                       │   Tether WDK Wallet      │
                       │                          │
                       │  @tetherto/wdk           │
                       │  wdk-wallet-evm          │
                       │  Polygon Amoy Testnet    │
                       │  Native MATIC transfers  │
                       └────────────┬─────────────┘
                                    │  real on-chain transactions
                       ┌────────────▼────────────┐
                       │  amoy.polygonscan.com    │
                       │  verifiable on-chain     │
                       └──────────────────────────┘
```

---

## The Four Agents

### 🎬 1. WatchTimeTipAgent
Fires when engagement crosses 50% of a video. **Grok** contextualises the tip amount based on depth — a 95% watch of a 45-minute video earns more than 70% of a 3-minute clip. Budget-aware: checks daily spend before executing.

### 💬 2. EmotionChatAgent
Maintains a rolling 60-second sentiment window over live chat. When crowd energy spikes — "LFG", "GOAT", "legendary" flooding in — **Grok** reads the room and rewards the creator proportionally. Emotion-driven, not rule-driven.

### 🏆 3. MilestoneTipAgent
Intercepts structured milestone events — `DEBATE_WIN`, `VIEWS_100K`, `SUBS_MILESTONE`. **Grok** generates a custom celebration message and sizes a premium tip. Every decision and its reasoning is logged and displayed live.

### ⚡ 4. SwarmAgent — The Headline Feature
Fans collectively pledge toward a goal: *"$100 pool if she hits 200K subs."* When the trigger fires, every participant's tip executes **simultaneously** via `asyncio.gather()`. One event. One AI-written announcement. Every fan tips at once.

```python
# backend/core/swarm_pool.py — coordinated parallel execution
results = await asyncio.gather(*[_tip_one(p) for p in participants])
# → 20 fans · 20 on-chain transactions · fired in < 1 second
```

---

## Real Blockchain Transactions

TipMind sends **real transactions** on Polygon Amoy testnet — verifiable on [amoy.polygonscan.com](https://amoy.polygonscan.com/address/0xeED09Ed732aE030D58F36235FF445012992b9CDC).

Every tip is a genuine signed EVM transaction, not a simulation. The wallet is funded with testnet MATIC. Tip amounts are scaled symbolically (1¢ per $1 USD) so the demo wallet doesn't run dry, but the infrastructure is identical to production Polygon mainnet.

To go live: swap `WDK_CHAIN=polygon`, fund the wallet with USDT, done.

---

## Autonomy — What Runs Without You

Once deployed, TipMind operates indefinitely without human input:

| What happens automatically | How |
|---|---|
| Polls 4+ YouTube channels every 90s | `YouTubePoller` asyncio background task |
| Detects new videos, generates events | RSS XML parsing — no API key needed |
| Fetches real view/like counts | YouTube Data API v3 (free tier) |
| Reads chat sentiment | `EmotionChatAgent` rolling 60s window |
| Detects milestone keywords in titles | Regex pattern matching |
| Sizes tips using AI reasoning | xAI Grok-3-mini via OpenAI-compatible API |
| Executes on-chain transfers | Tether WDK → Polygon |
| Broadcasts decisions over WebSocket | Live dashboard updates in real time |
| Logs every decision + reasoning | `agent_decisions_log` SQLite table |

**The only human action required: deploy it.**

---

## WDK Integration

TipMind uses a dedicated **Node.js microservice** (`wdk-service/`) to wrap Tether's `@tetherto/wdk` SDK — because WDK is Node.js-native and the core backend is Python.

```
Python FastAPI  ──HTTP──►  wdk-service (Node.js :3001)  ──WDK──►  Polygon
```

The service derives an EVM wallet from a BIP39 seed phrase, signs transactions via `ethers.js`, and broadcasts them through WDK's `account.sendTransaction()`.

```javascript
// wdk-service/index.js
const wdk = new WDK(SEED).registerWallet('polygon', WalletManagerEvm, { rpcUrl: RPC_URL });
const account = await wdk.getAccount('polygon', 0);

// Testnet: native MATIC via ethers.js HDNodeWallet (same key derivation)
const txResponse = await signer.sendTransaction({ to, value: valueWei, gasPrice, nonce });

// Mainnet: ERC-20 USDT via WDK
const { hash } = await account.sendTransaction({
  to: USDT_CONTRACT,          // 0xc2132D05D31c914a87C6611C10748AEb04B58e8F
  data: encodeUsdtTransfer(to, amount),
  value: '0',
});
```

**Fallback:** No seed phrase set? The backend switches to `MockWallet` automatically — same interface, fake SHA-256 tx hashes, simulated latency. Every other part of the stack behaves identically.

---

## Fan Swarms — Collective Action Design

A swarm is a **commitment mechanism** for collective fan action:

1. **Create** — Set a goal: trigger event + USD target
2. **Join** — Fans pledge amounts; bounded by `max_per_video` setting
3. **Trigger** — A real event fires (`DEBATE_WIN`, `SUBS_MILESTONE`, etc.)
4. **Release** — SwarmAgent calls `release_swarm()` → all tips fire simultaneously

Why this works economically:
- Commitments are soft-locked, not escrowed — no custody risk
- 24-hour TTL prevents indefinite lockup
- Grok writes the announcement *before* release — the creator sees the collective moment
- `asyncio.gather()` means all funds arrive within the same block window

---

## Quick Start

**Prerequisites:** Python 3.11+, Node.js 18+

```bash
# 1. Clone
git clone https://github.com/ex-plo-rer/TipMind.git
cd TipMind

# 2. Install everything (Python + Node + frontend)
make install

# 3. Configure
cp .env.example .env
# Add: ANTHROPIC_API_KEY or XAI_API_KEY
# For live tips: WDK_SEED_PHRASE + WDK_RPC_URL
# Optional: YOUTUBE_CHANNEL_IDS (defaults to MKBHD, t3.gg, Coin Bureau, Dave2D)

# 4. Launch
make dev
```

| Service | URL |
|---|---|
| Dashboard | http://localhost:3000 |
| Backend API + Swagger | http://localhost:8000/docs |
| WebSocket live feed | ws://localhost:8000/ws/feed |
| WDK microservice | http://localhost:3001/health |

**Full demo experience** — seeds data, opens browser, starts everything:
```bash
make demo
```

---

## Project Structure

```
TipMind/
├── backend/
│   ├── agents/
│   │   ├── tip_agent.py         # WatchTimeTipAgent — engagement-based micro-tips
│   │   ├── emotion_agent.py     # EmotionChatAgent — rolling sentiment detection
│   │   ├── milestone_agent.py   # MilestoneTipAgent — celebration tips + AI messages
│   │   └── swarm_agent.py       # SwarmAgent — parallel coordinated tip release
│   ├── api/
│   │   ├── routes.py            # REST endpoints (status, swarms, txns, demo, prefs)
│   │   └── websocket.py         # WebSocket feed — formatted real-time event stream
│   ├── core/
│   │   ├── event_bus.py         # asyncio pub/sub event bus + WebSocket broadcast
│   │   ├── groq_client.py       # xAI Grok-3-mini / Groq Llama-3.3 LLM client
│   │   ├── orchestrator.py      # Agent coordinator + system status
│   │   ├── poller.py            # Autonomous YouTube RSS poller
│   │   ├── swarm_pool.py        # Swarm lifecycle + asyncio.gather() release
│   │   ├── wallet.py            # WalletInterface → WDKWallet / MockWallet
│   │   └── youtube_client.py    # YouTube Data API v3 engagement stats
│   ├── data/
│   │   ├── models.py            # Pydantic + SQLAlchemy ORM models
│   │   └── database.py          # Async SQLAlchemy engine + session factory
│   ├── demo/
│   │   └── seed.py              # Demo seed: transactions, swarms, agent decisions
│   └── main.py                  # FastAPI app + lifespan (tables, seed, poller, agents)
├── frontend/
│   └── app/
│       └── page.tsx             # Dashboard: SWR polling + WebSocket + Framer Motion
├── wdk-service/
│   ├── index.js                 # Node.js WDK microservice (Express + ethers.js + WDK)
│   └── package.json             # @tetherto/wdk, wdk-wallet-evm, ethers, express
├── assets/                      # Screenshots
├── Makefile                     # install / dev / demo / seed / test / clean
├── Procfile                     # Railway deployment
├── nixpacks.toml                # Railway build config (Python + Node in one service)
├── start.sh                     # Single-container startup: WDK + FastAPI
├── DEMO_GUIDE.md                # 90-second judge walkthrough
└── .env.example                 # All environment variables documented
```

---

## API Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/status` | Agent states, wallet balance, active swarms, tips today |
| `GET` | `/api/metrics` | Totals, top creators, weekly breakdown |
| `GET` | `/api/transactions` | Paginated tip history with on-chain tx hashes |
| `GET` | `/api/decisions` | Agent decision log with full AI reasoning |
| `GET` | `/api/swarms` | Active swarm goals with progress bars |
| `POST` | `/api/swarms` | Create a swarm goal |
| `POST` | `/api/swarms/{id}/join` | Join a swarm with a pledged amount |
| `GET` | `/api/preferences` | User agent configuration |
| `PUT` | `/api/preferences` | Update preferences (budget, token, triggers) |
| `POST` | `/api/demo/{scenario}` | Inject demo event: `watch` · `hype` · `milestone` · `swarm` |
| `WS` | `/ws/feed` | Live agent decision stream (JSON frames) |

Full interactive docs at `/docs` (Swagger UI).

---

## Make Commands

```bash
make install   # pip install + npm install (backend + frontend + wdk-service)
make dev       # Start all 3 services — FastAPI :8000 · Next.js :3000 · WDK :3001
make demo      # Seed data → start all services → open browser
make seed      # Seed database with realistic demo data
make test      # pytest
make clean     # Remove tipmind.db, .next/, __pycache__/
```

---

## Environment Variables

| Variable | Required | Description |
|---|---|---|
| `XAI_API_KEY` | Recommended | xAI Grok-3-mini — primary LLM for all 4 agents |
| `ANTHROPIC_API_KEY` | Optional | Fallback Claude API key |
| `WDK_SEED_PHRASE` | For live tips | 12-word BIP39 mnemonic for WDK wallet |
| `WDK_RPC_URL` | For live tips | Polygon / Amoy RPC endpoint |
| `WDK_CHAIN` | No | `polygon`, `ethereum`, or `amoy` (default: `amoy`) |
| `WDK_API_KEY` | No | Shared secret between Python ↔ WDK service |
| `YOUTUBE_API_KEY` | No | YouTube Data API v3 — enables real engagement stats |
| `YOUTUBE_CHANNEL_IDS` | No | Comma-separated channel IDs (defaults to 4 public channels) |
| `MAX_TIP_PER_VIDEO` | No | Per-video tip cap in USD (default: `5.00`) |
| `DEFAULT_TOKEN` | No | `USDT` (default), `XAUT`, or `BTC` |

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python 3.12 · FastAPI · SQLAlchemy (async) · SQLite |
| AI | xAI Grok-3-mini (primary) · Groq Llama-3.3-70B (fallback) |
| Blockchain | Tether WDK · ethers.js · Polygon Amoy |
| Frontend | Next.js 16 · TypeScript · Framer Motion · SWR |
| Real-time | WebSocket (FastAPI native) · asyncio pub/sub |
| Data | YouTube RSS feeds · YouTube Data API v3 |
| Deployment | Railway (backend + WDK) · Vercel (frontend) |

---

<div align="center">

Built for the **Tether WDK Hackathon** · Powered by **xAI Grok** · On-chain with **WDK**

*Fans shouldn't have to remember to tip. TipMind remembers for them.*

</div>
