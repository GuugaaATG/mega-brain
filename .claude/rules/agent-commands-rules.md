---
paths: **/*
---

# AIOS Agent Commands

## Agent Activation Syntax

### @syntax (Primary)

| Command          | Agent            | Description             |
| ---------------- | ---------------- | ----------------------- |
| `@dev`           | Developer        | Full-stack development  |
| `@qa`            | QA Engineer      | Testing and quality     |
| `@architect`     | Architect        | System design           |
| `@pm`            | Product Manager  | Product strategy        |
| `@po`            | Product Owner    | Backlog management      |
| `@sm`            | Scrum Master     | Process facilitation    |
| `@analyst`       | Business Analyst | Requirements analysis   |
| `@devops`        | DevOps Engineer  | CI/CD, infrastructure   |
| `@data-engineer` | Data Engineer    | Data pipelines          |
| `@aios-master`   | AIOS Master      | Framework orchestration |

### Mega Brain Agents

| Command   | Agent                    | Description         |
| --------- | ------------------------ | ------------------- |
| `@cro`    | Chief Revenue Officer    | Revenue strategy    |
| `@cfo`    | Chief Financial Officer  | Financial strategy  |
| `@cmo`    | Chief Marketing Officer  | Marketing strategy  |
| `@coo`    | Chief Operations Officer | Operations strategy |
| `@closer` | Closer                   | Sales closing       |
| `@bdr`    | Business Dev Rep         | Lead qualification  |
| `@sds`    | Sales Dev Rep            | Outbound sales      |
| `@lns`    | Lead Nurture Specialist  | Lead nurturing      |

### DNA Agents (Personas)

| Command          | Agent         | Expertise                |
| ---------------- | ------------- | ------------------------ |
| `@cole-gordon`   | Cole Gordon   | Sales management, PTMA   |
| `@alex-hormozi`  | Alex Hormozi  | $100M Offers, Grand Slam |
| `@jeremy-miner`  | Jeremy Miner  | NEPQ, sales psychology   |
| `@jeremy-haynes` | Jeremy Haynes | Digital marketing        |

### Council Agents (Deliberação)

| Command                   | Agent               | Role            |
| ------------------------- | ------------------- | --------------- |
| `@advogado-do-diabo`      | Diabo 😈            | Decision attack |
| `@critico-metodologico`   | Critico 🔬          | Source audit    |
| `@sintetizador`           | Sintese 🎯          | Integration     |

**Activation:** Use `/conclave` to convene the full Council.

### Copy Squad (Copywriting)

| Command       | Agent       | Role                           |
| ------------- | ----------- | ------------------------------ |
| `@copy-chief` | Copy Chief  | Copywriting Orchestrator       |

**Tier System:**
- **Tier 0 (Diagnosis):** Hopkins, Schwartz, Collier
- **Tier 1 (Masters $500M+):** Halbert, Bencivenga, Ogilvy
- **Tier 2 (Systematizers):** Kennedy, Todd Brown
- **Tier 3 (Specialists):** Benson (VSL)
- **Tool:** Sugarman 30 Triggers

**Activation:** `@copy-chief` then use `*help` for commands.

### MMOS Squad (Mind Mapper OS)

| Command        | Agent             | Role                          |
| -------------- | ----------------- | ----------------------------- |
| `@mind-mapper` | Mind Mapper       | Cognitive Archaeologist       |
| `@cognitive-analyst` | Cognitive Analyst | 8-Layer DNA Analysis    |
| `@identity-analyst`  | Identity Analyst  | Core Identity Extraction |
| `@research-specialist` | Research Specialist | Source Discovery      |
| `@system-prompt-architect` | Prompt Architect | Production Prompts   |

**Purpose:** Industrial pipeline for cognitive architecture cloning (DNA Mental™)
**Activation:** `@mind-mapper` or `/map {nome}` to start mapping

### ETL Squad (Data Collection)

| Command           | Agent            | Role                    |
| ----------------- | ---------------- | ----------------------- |
| `@data-collector` | Data Collector   | ETL Orchestrator        |
| `@web-specialist` | Web Specialist   | Web Content Extraction  |
| `@youtube-specialist` | YouTube Specialist | Video Collection   |
| `@social-specialist`  | Social Specialist  | Social Media Data   |

**Purpose:** Blog/content collection utilities (100% success rate)
**Location:** `.aiox/squads/etl/`

---

## Task Commands

### \*prefix (Task Execution)

| Command            | Description                  |
| ------------------ | ---------------------------- |
| `*help`            | Show available commands      |
| `*create-story`    | Create new development story |
| `*create-task`     | Create new task definition   |
| `*create-agent`    | Create new agent             |
| `*task {name}`     | Execute specific task        |
| `*workflow {name}` | Run workflow                 |

### Agent-Specific Tasks

| Command                     | Agent      | Description           |
| --------------------------- | ---------- | --------------------- |
| `*dev-develop-story`        | @dev       | Develop a story       |
| `*qa-review-code`           | @qa        | Review code quality   |
| `*architect-analyze-impact` | @architect | Analyze change impact |
| `*pm-create-prd`            | @pm        | Create PRD            |

---

## Squad Commands

### \*squad-prefix

| Command                     | Description                      |
| --------------------------- | -------------------------------- |
| `*bilhon-squad start`       | Start BILHON document generation |
| `*bilhon-squad status`      | Check squad status               |
| `*bilhon-squad report {id}` | Get task report                  |

---

## MCP Operations (DevOps Only)

MCP management is handled EXCLUSIVELY by `@devops`:

| Command       | Description        |
| ------------- | ------------------ |
| `*search-mcp` | Search MCP catalog |
| `*add-mcp`    | Add MCP server     |
| `*list-mcps`  | List enabled MCPs  |
| `*remove-mcp` | Remove MCP server  |

Other agents are MCP **consumers**, not administrators.

---

## Database Operations (@data-engineer)

| Command               | Description         |
| --------------------- | ------------------- |
| `*db-bootstrap`       | Initialize database |
| `*db-apply-migration` | Apply migration     |
| `*db-rollback`        | Rollback migration  |
| `*db-seed`            | Seed test data      |
| `*db-schema-audit`    | Audit schema        |
| `*db-rls-audit`       | Audit RLS policies  |

---

## Workflow Execution

### Development Workflows

| Workflow               | Description                 |
| ---------------------- | --------------------------- |
| `greenfield-fullstack` | New full-stack project      |
| `greenfield-service`   | New backend service         |
| `greenfield-ui`        | New frontend project        |
| `brownfield-fullstack` | Enhance existing full-stack |
| `brownfield-service`   | Enhance existing backend    |
| `brownfield-ui`        | Enhance existing frontend   |

### Execution

```
*workflow greenfield-fullstack
```

---

## AIOS State Management

AIOS provides integrated state management:

| Behavior        | Description                       |
| --------------- | --------------------------------- |
| Auto-greeting   | Session start with status         |
| Memory tracking | Persistent memory across sessions |
| Routing         | Automatic agent suggestion        |
| Learning        | Behavioral pattern extraction     |

AIOS state is in `.claude/aios/STATE.json`.

---

## Quick Reference

| I want to...             | Command                  |
| ------------------------ | ------------------------ |
| Get help                 | `*help`                  |
| Activate dev agent       | `@dev`                   |
| Create a story           | `*create-story`          |
| Run a task               | `*task {name}`           |
| Start a workflow         | `*workflow {name}`       |
| Generate BILHON document | `*bilhon-squad start`    |
| Manage MCP               | Contact `@devops`        |
| Database operations      | Contact `@data-engineer` |

---

_AIOS Agent Commands v1.0 - Auto-injected by Claude Code_
