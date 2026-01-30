---
name: elite-longterm-memory
version: 1.0.0
description: "Ultimate AI agent memory system. Combines bulletproof WAL protocol, vector search, git-based knowledge graphs, cloud backup, and maintenance hygiene. Never lose context again. For Clawdbot, Moltbot, Claude, GPT agents."
author: NextFrontierBuilds
keywords: [memory, ai-agent, long-term-memory, vector-search, lancedb, git-notes, wal, persistent-context, claude, gpt, clawdbot, moltbot]
metadata:
  clawdbot:
    emoji: "🧠"
    requires:
      env:
        - OPENAI_API_KEY
      plugins:
        - memory-lancedb
---

# Elite Longterm Memory 🧠

**The ultimate memory system for AI agents.** Combines 6 proven approaches into one bulletproof architecture.

Never lose context. Never forget decisions. Never repeat mistakes.

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    ELITE LONGTERM MEMORY                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │
│  │   HOT RAM   │  │  WARM STORE │  │  COLD STORE │             │
│  │             │  │             │  │             │             │
│  │ SESSION-    │  │  LanceDB    │  │  Git-Notes  │             │
│  │ STATE.md    │  │  Vectors    │  │  Knowledge  │             │
│  │             │  │             │  │  Graph      │             │
│  │ (survives   │  │ (semantic   │  │ (permanent  │             │
│  │  compaction)│  │  search)    │  │  decisions) │             │
│  └─────────────┘  └─────────────┘  └─────────────┘             │
│         │                │                │                     │
│         └────────────────┼────────────────┘                     │
│                          ▼                                      │
│                  ┌─────────────┐                                │
│                  │  MEMORY.md  │  ← Curated long-term           │
│                  │  + daily/   │    (human-readable)            │
│                  └─────────────┘                                │
│                          │                                      │
│                          ▼                                      │
│                  ┌─────────────┐                                │
│                  │ SuperMemory │  ← Cloud backup (optional)     │
│                  │    API      │                                │
│                  └─────────────┘                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## The 5 Memory Layers

### Layer 1: HOT RAM (SESSION-STATE.md)
**From: bulletproof-memory**

Active working memory that survives compaction. Write-Ahead Log protocol.

```markdown
# SESSION-STATE.md — Active Working Memory

## Current Task
[What we're working on RIGHT NOW]

## Key Context
- User preference: ...
- Decision made: ...
- Blocker: ...

## Pending Actions
- [ ] ...
```

**Rule:** Write BEFORE responding. Triggered by user input, not agent memory.

### Layer 2: WARM STORE (LanceDB Vectors)
**From: lancedb-memory**

Semantic search across all memories. Auto-recall injects relevant context.

```bash
# Auto-recall (happens automatically)
memory_recall query="project status" limit=5

# Manual store
memory_store text="User prefers dark mode" category="preference" importance=0.9
```

### Layer 3: COLD STORE (Git-Notes Knowledge Graph)
**From: git-notes-memory**

Structured decisions, learnings, and context. Branch-aware.

```bash
# Store a decision (SILENT - never announce)
python3 memory.py -p $DIR remember '{"type":"decision","content":"Use React for frontend"}' -t tech -i h

# Retrieve context
python3 memory.py -p $DIR get "frontend"
```

### Layer 4: CURATED ARCHIVE (MEMORY.md + daily/)
**From: Clawdbot native**

Human-readable long-term memory. Daily logs + distilled wisdom.

```
workspace/
├── MEMORY.md              # Curated long-term (the good stuff)
└── memory/
    ├── 2026-01-30.md      # Daily log
    ├── 2026-01-29.md
    └── topics/            # Topic-specific files
```

### Layer 5: CLOUD BACKUP (SuperMemory) — Optional
**From: supermemory**

Cross-device sync. Chat with your knowledge base.

```bash
export SUPERMEMORY_API_KEY="your-key"
supermemory add "Important context"
supermemory search "what did we decide about..."
```

## Quick Setup

### 1. Create SESSION-STATE.md (Hot RAM)

```bash
cat > SESSION-STATE.md << 'EOF'
# SESSION-STATE.md — Active Working Memory

This file is the agent's "RAM" — survives compaction, restarts, distractions.

## Current Task
[None]

## Key Context
[None yet]

## Pending Actions
- [ ] None

## Recent Decisions
[None yet]

---
*Last updated: [timestamp]*
EOF
```

### 2. Enable LanceDB (Warm Store)

In `~/.clawdbot/clawdbot.json`:

```json
{
  "memorySearch": {
    "enabled": true,
    "provider": "openai",
    "sources": ["memory"],
    "minScore": 0.3,
    "maxResults": 10
  },
  "plugins": {
    "entries": {
      "memory-lancedb": {
        "enabled": true,
        "config": {
          "autoCapture": false,
          "autoRecall": true,
          "captureCategories": ["preference", "decision", "fact"],
          "minImportance": 0.7
        }
      }
    }
  }
}
```

### 3. Initialize Git-Notes (Cold Store)

```bash
cd ~/clawd
git init  # if not already
python3 skills/git-notes-memory/memory.py -p . sync --start
```

### 4. Verify MEMORY.md Structure

```bash
# Ensure you have:
# - MEMORY.md in workspace root
# - memory/ folder for daily logs
mkdir -p memory
```

### 5. (Optional) Setup SuperMemory

```bash
export SUPERMEMORY_API_KEY="your-key"
# Add to ~/.zshrc for persistence
```

## Agent Instructions

### On Session Start
1. Read SESSION-STATE.md — this is your hot context
2. Run `memory_search` for relevant prior context
3. Check memory/YYYY-MM-DD.md for recent activity

### During Conversation
1. **User gives concrete detail?** → Write to SESSION-STATE.md BEFORE responding
2. **Important decision made?** → Store in Git-Notes (SILENTLY)
3. **Preference expressed?** → `memory_store` with importance=0.9

### On Session End
1. Update SESSION-STATE.md with final state
2. Move significant items to MEMORY.md if worth keeping long-term
3. Create/update daily log in memory/YYYY-MM-DD.md

### Memory Hygiene (Weekly)
1. Review SESSION-STATE.md — archive completed tasks
2. Check LanceDB for junk: `memory_recall query="*" limit=50`
3. Clear irrelevant vectors: `memory_forget id=<id>`
4. Consolidate daily logs into MEMORY.md

## The WAL Protocol (Critical)

**Write-Ahead Log:** Write state BEFORE responding, not after.

| Trigger | Action |
|---------|--------|
| User states preference | Write to SESSION-STATE.md → then respond |
| User makes decision | Write to SESSION-STATE.md → then respond |
| User gives deadline | Write to SESSION-STATE.md → then respond |
| User corrects you | Write to SESSION-STATE.md → then respond |

**Why?** If you respond first and crash/compact before saving, context is lost. WAL ensures durability.

## Example Workflow

```
User: "Let's use Tailwind for this project, not vanilla CSS"

Agent (internal):
1. Write to SESSION-STATE.md: "Decision: Use Tailwind, not vanilla CSS"
2. Store in Git-Notes: decision about CSS framework
3. memory_store: "User prefers Tailwind over vanilla CSS" importance=0.9
4. THEN respond: "Got it — Tailwind it is..."
```

## Maintenance Commands

```bash
# Audit vector memory
memory_recall query="*" limit=50

# Clear all vectors (nuclear option)
rm -rf ~/.clawdbot/memory/lancedb/
clawdbot gateway restart

# Export Git-Notes
python3 memory.py -p . export --format json > memories.json

# Check memory health
du -sh ~/.clawdbot/memory/
wc -l MEMORY.md
ls -la memory/
```

## Troubleshooting

**Agent keeps forgetting mid-conversation:**
→ SESSION-STATE.md not being updated. Check WAL protocol.

**Irrelevant memories injected:**
→ Disable autoCapture, increase minImportance threshold.

**Memory too large, slow recall:**
→ Run hygiene: clear old vectors, archive daily logs.

**Git-Notes not persisting:**
→ Run `git notes push` to sync with remote.

---

## Links

- bulletproof-memory: https://clawdhub.com/skills/bulletproof-memory
- lancedb-memory: https://clawdhub.com/skills/lancedb-memory
- git-notes-memory: https://clawdhub.com/skills/git-notes-memory
- memory-hygiene: https://clawdhub.com/skills/memory-hygiene
- supermemory: https://clawdhub.com/skills/supermemory

---

*Built by [@NextXFrontier](https://x.com/NextXFrontier) — Part of the Next Frontier AI toolkit*
