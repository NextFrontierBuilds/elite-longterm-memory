# Elite Longterm Memory 🧠

**The ultimate memory system for AI agents.** Never lose context again.

Combines 6 proven memory approaches into one bulletproof architecture:

- ✅ **Bulletproof WAL Protocol** — Write-ahead logging survives compaction
- ✅ **LanceDB Vector Search** — Semantic recall of relevant memories
- ✅ **Git-Notes Knowledge Graph** — Structured decisions, branch-aware
- ✅ **File-Based Archives** — Human-readable MEMORY.md + daily logs
- ✅ **Cloud Backup** — Optional SuperMemory sync
- ✅ **Memory Hygiene** — Keep vectors lean, prevent token waste

## Quick Start

```bash
# Initialize in your workspace
npx elite-longterm-memory init

# Check status
npx elite-longterm-memory status

# Create today's log
npx elite-longterm-memory today
```

## Architecture

```
┌─────────────────────────────────────────────────────┐
│              ELITE LONGTERM MEMORY                  │
├─────────────────────────────────────────────────────┤
│  HOT RAM          WARM STORE        COLD STORE     │
│  SESSION-STATE.md → LanceDB      → Git-Notes       │
│  (survives         (semantic       (permanent      │
│   compaction)       search)         decisions)     │
│         │              │                │          │
│         └──────────────┼────────────────┘          │
│                        ▼                           │
│                   MEMORY.md                        │
│               (curated archive)                    │
└─────────────────────────────────────────────────────┘
```

## The 5 Memory Layers

| Layer | File/System | Purpose | Persistence |
|-------|-------------|---------|-------------|
| 1. Hot RAM | SESSION-STATE.md | Active task context | Survives compaction |
| 2. Warm Store | LanceDB | Semantic search | Auto-recall |
| 3. Cold Store | Git-Notes | Structured decisions | Permanent |
| 4. Archive | MEMORY.md + daily/ | Human-readable | Curated |
| 5. Cloud | SuperMemory | Cross-device sync | Optional |

## The WAL Protocol

**Critical insight:** Write state BEFORE responding, not after.

```
User: "Let's use Tailwind for this project"

Agent (internal):
1. Write to SESSION-STATE.md → "Decision: Use Tailwind"
2. THEN respond → "Got it — Tailwind it is..."
```

If you respond first and crash before saving, context is lost. WAL ensures durability.

## For Clawdbot/Moltbot Users

Add to `~/.clawdbot/clawdbot.json`:

```json
{
  "memorySearch": {
    "enabled": true,
    "provider": "openai",
    "sources": ["memory"]
  }
}
```

## Files Created

```
workspace/
├── SESSION-STATE.md    # Hot RAM (active context)
├── MEMORY.md           # Curated long-term memory
└── memory/
    ├── 2026-01-30.md   # Daily logs
    └── ...
```

## Commands

```bash
elite-memory init      # Initialize memory system
elite-memory status    # Check health
elite-memory today     # Create today's log
elite-memory help      # Show help
```

## Links

- [Full Documentation (SKILL.md)](./SKILL.md)
- [ClawdHub](https://clawdhub.com/skills/elite-longterm-memory)
- [GitHub](https://github.com/NextFrontierBuilds/elite-longterm-memory)

---

Built by [@NextXFrontier](https://x.com/NextXFrontier)
