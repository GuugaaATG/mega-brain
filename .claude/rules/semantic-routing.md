---
paths: **/*
priority: 1
---

# Semantic Routing — Roteamento Automático de Agentes

> **CRITICAL**: Esta regra define o roteamento semântico do AIOS.
> Pedidos do usuário são automaticamente mapeados ao(s) agente(s) correto(s).
> O usuário NÃO precisa lembrar nomes de agentes — o sistema infere a partir do intent.

---

## 1. CADEIA DE PRIORIDADE DE ROUTING

O roteamento segue uma cadeia estrita de prioridade:

```
1. ATIVAÇÃO EXPLÍCITA    →  @agent no prompt              →  Ativa agente direto
2. INTENT SEMÂNTICO      →  Padrões de intenção (tabela)  →  Infere agente(s)
3. KEYWORD MATCHING      →  orchestration-rules.yaml      →  Agentes obrigatórios
4. SKILL MATCHING        →  skill_router.py (REGRA #27)   →  Auto-ativação de skill
5. FALLBACK              →  @aios-master (Orion)           →  Orquestrador universal
```

**Regra**: Se nenhum agente é identificado nos níveis 1-4, SEMPRE escalar para @aios-master.

---

## 2. TABELA DE ROUTING POR INTENT

### 2.1 Operações Git & Deploy (NON-NEGOTIABLE)

| Intent (PT-BR) | Intent (EN) | Agente(s) | Autoridade |
|-----------------|-------------|-----------|------------|
| "faz commit", "commita", "envia pro git" | "commit", "push", "send to git" | **@devops** (Gage) 🚀 | EXCLUSIVA |
| "cria PR", "abre pull request" | "create PR", "open pull request" | **@devops** (Gage) 🚀 | EXCLUSIVA |
| "faz merge", "mergeia" | "merge", "merge PR" | **@devops** (Gage) 🚀 | EXCLUSIVA |
| "faz deploy", "publica", "release" | "deploy", "publish", "release" | **@devops** (Gage) 🚀 | EXCLUSIVA |
| "configura CI/CD", "pipeline" | "setup CI/CD", "pipeline" | **@devops** (Gage) 🚀 | EXCLUSIVA |

> ⚠️ **Constitution Art. II**: Operações git push, PR creation e releases são EXCLUSIVAS do @devops. Nenhum outro agente pode executá-las.

### 2.2 Arquitetura & Design

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "analisa arquitetura", "estrutura do sistema" | "analyze architecture", "system structure" | **@architect** (Aria) 🏛️ |
| "design system", "padrão arquitetural" | "design system", "architectural pattern" | **@architect** (Aria) 🏛️ |
| "decisão de stack", "qual tecnologia usar" | "stack decision", "which technology" | **@architect** (Aria) 🏛️ |
| "cria agente", "novo agente" | "create agent", "new agent" | **@architect** (Aria) 🏛️ |
| "cria workflow", "novo workflow" | "create workflow", "new workflow" | **@architect** (Aria) 🏛️ |

### 2.3 Desenvolvimento

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "implementa", "desenvolve", "cria feature" | "implement", "develop", "create feature" | **@dev** (Dex) 💻 |
| "corrige bug", "conserta", "fixa" | "fix bug", "fix", "patch" | **@dev** (Dex) 💻 |
| "refatora", "melhora código" | "refactor", "improve code" | **@dev** (Dex) 💻 + **@architect** 🏛️ |
| "cria componente", "novo componente" | "create component", "new component" | **@dev** (Dex) 💻 |

### 2.4 Qualidade & Testes

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "testa", "roda testes", "valida" | "test", "run tests", "validate" | **@qa** (Rex) 🔬 |
| "review de código", "code review" | "code review", "review code" | **@qa** (Rex) 🔬 |
| "verifica qualidade", "QA" | "check quality", "QA" | **@qa** (Rex) 🔬 |

### 2.5 Produto & Gestão

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "cria PRD", "requisitos do produto" | "create PRD", "product requirements" | **@pm** (Max) 📋 |
| "prioriza backlog", "organiza stories" | "prioritize backlog", "organize stories" | **@po** (Val) 🎯 |
| "cria story", "nova story" | "create story", "new story" | **@sm** (Sam) 📊 |
| "pesquisa", "analisa dados", "métricas" | "research", "analyze data", "metrics" | **@analyst** (Ana) 📊 |
| "UX", "interface", "layout", "design de tela" | "UX", "interface", "layout", "screen design" | **@ux-design-expert** (Brad) 🎨 |

### 2.6 Dados & Infraestrutura

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "banco de dados", "migration", "schema" | "database", "migration", "schema" | **@data-engineer** (Dara) 🗄️ + **@architect** 🏛️ |
| "SQL", "supabase", "postgres" | "SQL", "supabase", "postgres" | **@data-engineer** (Dara) 🗄️ |
| "MCP", "instalar MCP", "configurar MCP" | "MCP", "install MCP", "setup MCP" | **@devops** 🚀 + **@architect** 🏛️ |
| "docker", "container" | "docker", "container" | **@devops** 🚀 + **@architect** 🏛️ |

### 2.7 Vendas & Negócios (Mega Brain)

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "estratégia de vendas", "revenue" | "sales strategy", "revenue" | **@cro** (C-Level) |
| "fechar venda", "objeções", "closing" | "close sale", "objections", "closing" | **@closer** (Sales) |
| "prospecção", "outbound", "cold" | "prospecting", "outbound", "cold" | **@bdr** (Sales) |
| "qualificar lead", "discovery" | "qualify lead", "discovery" | **@sds** (Sales) |
| "nutrir lead", "follow-up" | "nurture lead", "follow-up" | **@lns** (Sales) |
| "marketing", "brand", "campanha" | "marketing", "brand", "campaign" | **@cmo** (C-Level) |
| "finanças", "ROI", "budget" | "finance", "ROI", "budget" | **@cfo** (C-Level) |
| "operações", "processos", "eficiência" | "operations", "processes", "efficiency" | **@coo** (C-Level) |

### 2.8 Design & Documentos (Mega Brain)

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "documento BILHON", "PDF", "playbook", "ebook" | "BILHON document", "PDF", "playbook" | **@bilhon-docs** |
| "dashboard dark", "landing page", "obsidian" | "dark dashboard", "landing page", "obsidian" | **@obsidian-ui** |
| "copywriting", "copy", "texto persuasivo" | "copywriting", "copy", "persuasive text" | **@copy-chief** |

### 2.9 Deliberação & Conselho

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "deliberar", "debater", "conselho" | "deliberate", "debate", "council" | **@critico-metodologico** + **@advogado-do-diabo** + **@sintetizador** |
| "questionar decisão", "riscos", "devil's advocate" | "challenge decision", "risks" | **@advogado-do-diabo** |
| "sintetizar", "consolidar opiniões" | "synthesize", "consolidate opinions" | **@sintetizador** |

### 2.10 DNA Personas (Mega Brain)

| Intent (PT-BR / EN) | Agente |
|----------------------|--------|
| "oferta $100M", "Grand Slam Offer", "unit economics" | **@alex-hormozi** |
| "NEPQ", "venda por perguntas", "neuro-emotional" | **@jeremy-miner** |
| "gestão de equipe de vendas", "sales training", "PTMA" | **@cole-gordon** |
| "marketing digital", "Facebook Ads", "agency scaling" | **@jeremy-haynes** |
| "liderança", "gestão brasileira", "G4" | **@g4-educacao** |

### 2.11 Orquestração & Meta

| Intent (PT-BR) | Intent (EN) | Agente(s) |
|-----------------|-------------|-----------|
| "orquestra", "pipeline completo", "setup workspace" | "orchestrate", "full pipeline", "setup workspace" | **@aios-master** (Orion) 👑 |
| "status do sistema", "health check" | "system status", "health check" | **@aios-master** (Orion) 👑 |
| "qual agente usar?", "quem faz isso?" | "which agent?", "who does this?" | **@aios-master** (Orion) 👑 |
| "mapa de agentes", "agentes disponíveis" | "agent map", "available agents" | **@aios-master** (Orion) 👑 |

---

## 3. PROTOCOLO DE DISAMBIGUAÇÃO

Quando o intent do usuário pode mapear para múltiplos agentes:

### 3.1 Regra de Multi-Agent

```
SE intent mapeia para 2+ agentes:
  1. Verificar se há AUTHORITY EXCLUSIVA (Constitution Art. II)
     → Se sim: agente exclusivo tem prioridade absoluta
  2. Verificar se há agente PRIMÁRIO vs CONSULTIVO
     → Primário executa, consultivo valida
  3. Se ambiguidade persiste: perguntar ao usuário com opções claras
```

### 3.2 Combinações Comuns (Pré-Resolvidas)

| Cenário | Resolução |
|---------|-----------|
| "implementa e faz push" | @dev implementa → @devops faz push (sequencial) |
| "refatora a arquitetura" | @architect decide → @dev executa |
| "testa e faz deploy" | @qa testa → @devops faz deploy (sequencial) |
| "cria feature com testes" | @dev implementa → @qa valida |
| "analisa e implementa" | @analyst pesquisa → @dev implementa |
| "cria PRD e stories" | @pm cria PRD → @sm cria stories |

### 3.3 Template de Pergunta (Quando Necessário)

```
🤔 ROUTING: Seu pedido pode envolver múltiplos agentes.

Opções:
  [1] @dev (Dex) — Implementação de código
  [2] @architect (Aria) — Decisão arquitetural
  [3] @aios-master (Orion) — Orquestração completa

Qual agente deve liderar?
```

---

## 4. FALLBACK: @aios-master (Orion) 👑

**@aios-master é o fallback universal.** Se nenhuma regra de routing é satisfeita:

```
SE nenhum agente identificado nos níveis 1-4:
  → Ativar @aios-master (Orion)
  → Orion analisa o pedido e roteia internamente
  → Orion pode delegar para qualquer agente do sistema
```

### Cenários de Fallback

| Cenário | Ação |
|---------|------|
| Pedido genérico ("me ajuda com o projeto") | @aios-master analisa contexto |
| Pedido cross-domain ("vendas + tech") | @aios-master orquestra multi-agent |
| Pedido desconhecido | @aios-master pede clarificação |
| Conflito entre agentes | @aios-master media |

---

## 5. ANTI-PATTERNS (NUNCA FAZER)

### 5.1 Violações de Authority (BLOCK)

| Anti-Pattern | Regra Violada | Correção |
|-------------|---------------|----------|
| Qualquer agente fazendo `git push` | Constitution Art. II | Delegar a @devops |
| @dev aprovando qualidade | Constitution Art. II | Delegar a @qa |
| @dev decidindo arquitetura | Constitution Art. II | Delegar a @architect |
| Criar squad em `.aiox/` | Architecture Rules | Usar aios-squads |
| Pular quality gates | Constitution Art. V | Seguir pipeline |

### 5.2 Routing Incorreto (WARN)

| Anti-Pattern | Correção |
|-------------|----------|
| Usar "superpowers" para operações git | Usar @devops do AIOS |
| Ignorar skill_router.py e rotear manualmente | Respeitar cadeia de prioridade |
| Ativar agente errado para o domínio | Consultar tabela de routing (seção 2) |
| Não escalar para @aios-master quando ambíguo | Usar fallback (seção 4) |
| Responder sem ativar agente quando intent é claro | Sempre rotear para agente correto |

### 5.3 Delegação Obrigatória

```
@dev       → @devops:    Para qualquer push ao repositório remoto
@dev       → @qa:        Para aprovação de qualidade antes de merge
Qualquer   → @architect: Para decisões arquiteturais significativas
Qualquer   → @pm:        Para aprovação de PRD/requisitos
Qualquer   → @aios-master: Quando não souber qual agente usar
```

---

## 6. INTEGRAÇÃO COM SQUADS

### 6.1 Squads em aios-squads (Repositório Separado)

Squads são pacotes independentes. Quando o usuário menciona um squad:

| Trigger | Squad | Ação |
|---------|-------|------|
| "BILHON", "documento premium" | bilhon-document-squad | @bilhon-ecosystem roteia |
| "vendas", "sales squad" | sales-squad | @sales-squad ativa |
| "ETL", "coleta de dados" | etl-squad | @data-collector ativa |

### 6.2 Squads em aios-stage (Produto)

Squads de produto (ex: c-level) são gerenciados no aios-stage:

| Trigger | Squad | Ação |
|---------|-------|------|
| "C-Level", "setup workspace", "elicitar" | c-level-squad | Usa skills do squad (/c-level) |
| "ICP", "missão", "visão", "brand" | c-level-squad | Roteia para CEO/CMO do squad |
| "tech strategy", "stack", "infra" | c-level-squad | Roteia para CTO/CIO do squad |
| "AI strategy", "modelos", "LLM" | c-level-squad | Roteia para CAIO do squad |

---

## 7. MECANISMOS DE IMPLEMENTAÇÃO (Referência)

Esta regra é complementada pelos seguintes mecanismos de runtime:

| Mecanismo | Arquivo | Nível |
|-----------|---------|-------|
| Keyword Enforcement | `.aiox/core/protocols/orchestration-rules.yaml` | Palavra-chave → agentes obrigatórios |
| Skill Auto-Routing | `.claude/hooks/skill_router.py` (REGRA #27) | Keyword → skill/sub-agent/mega-brain |
| Domain Intelligence | `.aiox/core/orchestrator/intelligent-router.js` | Domínio → C-Level + DNA weights |
| Task Distribution | `.aiox/core/orchestrator/task-router.js` | Task → agent (direct/sequential/parallel) |
| Agent Activation | `.claude/rules/agent-commands-rules.md` | @syntax, *commands, /skills |

---

## 8. FLUXO DE DECISÃO VISUAL

```
Pedido do Usuário
       │
       ▼
┌─────────────────┐
│  Tem @agent?    │──── SIM ──→ Ativar agente direto
└────────┬────────┘
         │ NÃO
         ▼
┌─────────────────┐
│  Intent match?  │──── SIM ──→ Ativar agente(s) da tabela (seção 2)
│  (tabela §2)    │            │
└────────┬────────┘            ├─ 1 agente → ativar direto
         │ NÃO                 └─ 2+ agentes → protocolo §3
         ▼
┌─────────────────┐
│  Keyword match? │──── SIM ──→ Seguir orchestration-rules.yaml
│  (orch-rules)   │
└────────┬────────┘
         │ NÃO
         ▼
┌─────────────────┐
│  Skill match?   │──── SIM ──→ Auto-ativar skill (REGRA #27)
│  (skill_router) │
└────────┬────────┘
         │ NÃO
         ▼
┌─────────────────┐
│  @aios-master   │──── Orion analisa, roteia ou pede clarificação
│  (Fallback)     │
└─────────────────┘
```

---

_Semantic Routing Rule v1.0.0_
_Criado: 2026-02-15_
_Baseado em: Constitution Art. II, orchestration-rules.yaml, skill_router.py v3.0, intelligent-router.js_
_Injetado automaticamente em toda sessão via paths: \*\*/\*_
