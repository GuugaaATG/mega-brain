---
paths: **/*
---

# AIOS Architecture Rules

## CRITICAL: Repository Separation

AIOS operates with clear separation between framework core and squads. NEVER mix them:

| Component       | Purpose                                  | Location                   |
| --------------- | ---------------------------------------- | -------------------------- |
| **Framework**   | Core agents, tasks, workflows            | `.aiox/`              |
| **Squads**      | Multi-agent squads (packages)            | `../aios-squads/packages/` |

### SQUADS Location

**SQUADS go in `aios-squads/packages/` - NEVER in `.aiox/`.**

```
aios-squads/
├── packages/
│   ├── bilhon-document-squad/
│   │   ├── config.yaml
│   │   ├── package.json
│   │   ├── README.md
│   │   ├── agents/
│   │   ├── tasks/
│   │   └── templates/
│   └── {other-squad}/
└── registry.json
```

### Framework Structure

```
.aiox/
├── core/                    # Runtime engine
│   ├── config/              # Configuration loaders
│   ├── elicitation/         # Interactive workflows
│   ├── session/             # Session management
│   ├── protocols/           # Core protocols (constitution, agent-index)
│   └── orchestrator/        # Core orchestration
├── development/             # Development assets
│   ├── agents/              # Individual agent definitions (.md)
│   │   ├── dev.md
│   │   ├── qa.md
│   │   ├── architect.md
│   │   ├── pm.md
│   │   ├── po.md
│   │   ├── sm.md
│   │   ├── analyst.md
│   │   ├── devops.md
│   │   ├── data-engineer.md
│   │   ├── aios-master.md
│   │   └── mega-brain/      # Mega Brain agents (CARGO, PERSON)
│   ├── tasks/               # Task definitions (.md)
│   ├── workflows/           # Multi-step workflows (.yaml)
│   ├── skills/              # Reusable skills
│   └── agent-teams/         # Team compositions
├── data/                    # Framework data
│   ├── knowledge/           # DNA, playbooks, dossiers
│   └── learned-patterns.yaml
├── design-systems/          # Design system packages
│   └── BILHON/              # BILHON Design System
├── infrastructure/          # Scripts and integrations
│   └── scripts/
│       └── rag/             # RAG pipeline
└── squads/                  # DEPRECATED - use aios-squads repo
```

---

## File Placement Rules

### Agents

| Type                        | Location                                            | Format               |
| --------------------------- | --------------------------------------------------- | -------------------- |
| Core agents (dev, qa, etc.) | `.aiox/development/agents/{id}.md`             | Markdown             |
| Mega Brain CARGO agents     | `.aiox/development/agents/mega-brain/c-level/` | Folder with AGENT.md |
| Mega Brain PERSON agents    | `.aiox/data/knowledge/dna/{person-id}/`        | DNA package          |

### Tasks

| Type                 | Location                                           | Format   |
| -------------------- | -------------------------------------------------- | -------- |
| Framework tasks      | `.aiox/development/tasks/{name}.md`           | Markdown |
| Agent-specific tasks | `.aiox/development/tasks/{agent}-{action}.md` | Markdown |

### Workflows

| Type                  | Location                                       | Format     |
| --------------------- | ---------------------------------------------- | ---------- |
| Development workflows | `.aiox/development/workflows/{name}.yaml` | YAML       |
| Orchestrator modules  | `.aiox/core/orchestrator/{name}.js`       | JavaScript |

### Configuration

| Type           | Location                                      | Format |
| -------------- | --------------------------------------------- | ------ |
| Core protocols | `.aiox/core/protocols/*.yaml`            | YAML   |
| Agent index    | `.aiox/core/protocols/agent-index.yaml`  | YAML   |
| Constitution   | `.aiox/core/protocols/constitution.yaml` | YAML   |
| AIOS state     | `.claude/aios/STATE.json`                     | JSON   |

### Knowledge

| Type         | Location                                       | Format   |
| ------------ | ---------------------------------------------- | -------- |
| DNA packages | `.aiox/data/knowledge/dna/{id}/`          | Folder   |
| Playbooks    | `.aiox/data/knowledge/playbooks/{name}/`  | Folder   |
| Dossiers     | `.aiox/data/knowledge/dossiers/personas/` | Markdown |
| Batch logs   | `.aiox/data/knowledge/batches/`           | Markdown |

---

## Agent Activation

Agents are activated via:

1. **@syntax**: `@dev`, `@qa`, `@architect`, `@pm`, `@closer`, etc.
2. **Slash commands**: `/dev`, `/architect`, etc.
3. **Direct reference**: "use the dev agent"

The skill router hook detects keywords and loads appropriate agent context.

---

## CRITICAL: Hook-Managed Data Lives in .claude/ (NON-NEGOTIABLE)

**Princípio:** Tudo que hooks do Claude puxam ou inserem automaticamente DEVE viver dentro de `.claude/`. Dados gerenciados por hooks NUNCA ficam dentro de `.aiox/` ou outros diretórios de código.

### Regra

| Dado | Gerenciado por | Location | NUNCA em |
|------|---------------|----------|----------|
| Agent Memory (READ) | `skill_router.py` (UserPromptSubmit) | `.claude/agent-memory/{agent-slug}/MEMORY.md` | `.aiox/development/agents/` |
| Agent Memory (HINTS) | `memory_hints_injector.py` (UserPromptSubmit) | `.claude/agent-memory/{agent-slug}/MEMORY.md` | `.aiox/development/agents/` |
| Agent Memory (WRITE/batch) | `post_batch_cascading.py` (PostToolUse) | `.claude/agent-memory/{agent-slug}/MEMORY.md` | `.aiox/development/agents/` |
| Agent Memory (WRITE/session) | `agent_memory_persister.py` (SessionEnd) | `.claude/agent-memory/{agent-slug}/MEMORY.md` | `.aiox/development/agents/` |
| AIOS State | `session_start.py` / `session_end.py` | `.claude/aios/STATE.json` | qualquer outro lugar |
| Session Data | `session_autosave_v2.py` | `.claude/sessions/` | qualquer outro lugar |
| Routing Logs | `skill_router.py` | `.claude/aios/routing.jsonl` | qualquer outro lugar |
| Learning Data | `pattern_analyzer.py` | `.claude/learning-system/` | qualquer outro lugar |

### Agent Memory Convention

Cada agente do sistema tem uma pasta de memória em `.claude/agent-memory/`:

```
.claude/agent-memory/
├── aios-dev/MEMORY.md           # Core agents (prefixo aios-)
├── aios-qa/MEMORY.md
├── aios-architect/MEMORY.md
├── ceo/MEMORY.md                # C-Level agents (slug direto)
├── cro/MEMORY.md
├── nero-lead/MEMORY.md          # NERO squad (prefixo nero-)
├── alex-hormozi/MEMORY.md       # Personas (nome direto)
├── closer/MEMORY.md             # Sales agents (slug direto)
├── doc-master/MEMORY.md         # Doc pipeline (prefixo doc-)
├── advogado-do-diabo/MEMORY.md  # Council (slug direto)
└── ...                          # 87 agentes total
```

### Naming Convention

| Tipo | Padrão | Exemplo |
|------|--------|---------|
| Core agents | `aios-{role}` | `aios-dev`, `aios-qa`, `aios-architect` |
| C-Level | `{role}` | `ceo`, `cro`, `cfo`, `cmo` |
| NERO squad | `nero-{role}` | `nero-lead`, `nero-architect` |
| Sales | `{role}` | `closer`, `bdr`, `sds` |
| Personas | `{name}` | `alex-hormozi`, `cole-gordon` |
| Doc pipeline | `doc-{role}` | `doc-master`, `doc-enricher` |
| Council | `{slug}` | `advogado-do-diabo`, `sintetizador` |

### Agent Memory Lifecycle (Complete)

```
Activation (UserPromptSubmit)
  └── skill_router.py → READS .claude/agent-memory/{slug}/MEMORY.md
        ↓
Memory Hints (UserPromptSubmit, after skill_router)
  └── memory_hints_injector.py → INJECTS bracket-aware memory hints (MIS)
        ↓
Session Active (PostToolUse)
  └── post_batch_cascading.py → WRITES batch learnings to MEMORY.md
        ↓
Session End (SessionEnd)
  └── agent_memory_persister.py → WRITES session summary to MEMORY.md
        ↓
Next Activation → skill_router.py loads updated memory
```

### Regras de Enforcement

1. **READ**: `skill_router.py` loads `.claude/agent-memory/{slug}/MEMORY.md` on agent activation
2. **WRITE (batch)**: `post_batch_cascading.py` writes batch learnings during session
3. **WRITE (session)**: `agent_memory_persister.py` writes session summary at session end
4. **Tracking**: `post_tool_use.py` sets `session.agent_active` in STATE.json (used by persister)
5. **Novos agentes** criados DEVEM ter sua pasta em `.claude/agent-memory/` criada junto
6. **NUNCA** armazenar memória de runtime dentro dos arquivos de definição do agente (`.aiox/development/agents/`)
7. **Separação clara:** Definição do agente (`.aiox/`) vs Memória de runtime (`.claude/agent-memory/`)

### Justificativa

```
.aiox/development/agents/   = QUEM o agente É (definição, DNA, skills)
.claude/agent-memory/             = O QUE o agente APRENDEU (memória de sessão)
```

A definição é estática e versionada. A memória é dinâmica e gerenciada por hooks.

---

## CLAUDE.md File Policy (NON-NEGOTIABLE)

**CLAUDE.md files are strictly controlled.** The `claude-md-management` plugin is DISABLED and a PreToolUse hook (`claude_md_guard.py`) enforces this policy.

### Sanctioned CLAUDE.md Locations

| Path | Purpose |
|------|---------|
| `CLAUDE.md` | Repository root instructions |
| `.claude/CLAUDE.md` | Project-level instructions (main) |

**ALL other CLAUDE.md files are BLOCKED by the guard hook.**

### Enforcement

| Layer | Mechanism | Action |
|-------|-----------|--------|
| Plugin | `claude-md-management` disabled in `~/.claude/settings.json` | Prevents auto-creation |
| Hook | `.claude/hooks/claude_md_guard.py` (PreToolUse) | BLOCKS Write/Edit to unsanctioned CLAUDE.md |
| Convention | Architecture rules (this document) | Documentation reference |

### What Goes Where (Memory Architecture)

```
CLAUDE.md (root)                        = Project instructions (static, versioned)
.claude/CLAUDE.md                       = Project-level Claude instructions
.claude/agent-memory/{slug}/MEMORY.md   = Agent runtime memory (dynamic, hooks)
.claude/aios/STATE.json                 = Session state (dynamic, hooks)
.claude/learning-system/                = Learning captures (dynamic, hooks)
```

### NEVER Do

- ❌ Create CLAUDE.md in code directories (`.aiox/`, `src/`, etc.)
- ❌ Create CLAUDE.md in data directories (`.aiox/data/`, `aios-squads/`)
- ❌ Use CLAUDE.md for agent memory (use `.claude/agent-memory/`)
- ❌ Enable `claude-md-management` plugin (it creates CLAUDE.md spam)
- ❌ Write `<claude-mem-context>` tags anywhere

### History

- **2026-02-15:** Removed 338 auto-generated CLAUDE.md spam files, cleaned 19 with real content, added guard hook.
- **Root cause:** `claude-md-management@claude-plugins-official` plugin was creating CLAUDE.md in every directory Claude touched.

---

## AIOS State Management

The development framework provides state management through the orchestrator:

- Session management (STATE.json)
- Memory persistence (memory.db)
- Routing logs (routing.jsonl)
- Learning system (behavioral-learnings.yaml)

AIOS state is stored in `.claude/aios/`.

---

## Squad Creation Checklist

When creating a new squad:

1. Create in `aios-squads/packages/{squad-name}/`
2. Required files:
   - `config.yaml` - Squad manifest
   - `package.json` - NPM package definition
   - `README.md` - Documentation
3. Update `aios-squads/registry.json`
4. NEVER create squads in `.aiox/`

---

## Design System Placement

Design systems go in `.aiox/design-systems/`:

```
.aiox/design-systems/
├── BILHON/
│   ├── ARCHITECTURE.md
│   ├── SKILL.md
│   ├── shared/
│   ├── source/
│   └── documents/
└── {other-design-system}/
```

---

## Reserved Ports

| Port | Service | Owner | Description |
|------|---------|-------|-------------|
| **4000** | RAG Monitor Dashboard | NERO Squad | **RESERVADO** - Monitoramento contínuo de pipeline |
| 3000 | Next.js Dashboard | apps/dashboard | Dashboard principal AIOS |

### Port 4000 - NERO Visual Monitor

```
┌──────────────────────────────────────────────────────────────┐
│  📊 VISUAL MONITOR: http://localhost:4000                    │
│  → Acompanhe o processamento em tempo real na dashboard      │
│  → Eventos sendo enviados via WebSocket                      │
│  → RESERVADO: NERO Squad monitoring only                     │
└──────────────────────────────────────────────────────────────┘
```

**REGRAS:**
- Porta 4000 é **EXCLUSIVA** para monitoramento NERO
- Outros serviços devem usar portas diferentes (4001, 4002, etc.)
- Servidor: `aios-squads/packages/rag-monitor-squad/scripts/start.sh`
- Endpoint: POST `/events/rag` para enviar eventos
- WebSocket: `ws://localhost:4000/stream` para real-time updates

---

## Quick Reference

| I want to...         | Go to...                               |
| -------------------- | -------------------------------------- |
| Create an agent      | `.aiox/development/agents/`       |
| Create a task        | `.aiox/development/tasks/`        |
| Create a workflow    | `.aiox/development/workflows/`    |
| Create a squad       | `aios-squads/packages/`                |
| Add DNA knowledge    | `.aiox/data/knowledge/dna/`       |
| Add a playbook       | `.aiox/data/knowledge/playbooks/` |
| Configure AIOS state | `.claude/aios/`                        |
| Add agent memory     | `.claude/agent-memory/{slug}/`         |
| Add a skill          | `.aiox/development/skills/`       |
| Add design system    | `.aiox/design-systems/`           |

---

_AIOS Architecture Rules v1.4 - Cleaned project references (2026-03-03)_
