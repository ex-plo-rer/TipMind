TipMind is a fully autonomous AI agent that monitors YouTube creators, interprets engagement signals, and executes crypto tips on-chain — with zero human input.

Four specialised agents run continuously:

- **WatchTimeTipAgent** — tips when a user watches ≥50% of a video
- **EmotionChatAgent** — detects live chat sentiment spikes and rewards the moment
- **MilestoneTipAgent** — fires premium tips on DEBATE_WIN, VIEWS_100K, SUBS_MILESTONE
- **SwarmAgent** — rallies fans into a collective pool; releases all tips simultaneously via asyncio.gather()

Every tip decision is made by xAI Grok-3-mini, which sizes amounts, reads crowd energy, and writes custom announcements. Tips execute as real on-chain transactions via Tether's WDK SDK on Polygon — verifiable on-chain.

Stack: FastAPI · Next.js · SQLite · xAI Grok · Tether WDK · ethers.js · Polygon Amoy

Set it up once. It runs forever. Creators earn. Fans contribute. Zero friction.
