---
name: process-bilhon-inbox
description: "Processa arquivos no INBOX BILHON, classifica, extrai insights, e move para pasta destino. Use quando: novos arquivos em 00-INBOX, processar conteúdo BILHON, atualizar contexto da empresa."
---

# Process BILHON Inbox

Pipeline de processamento de conteúdo interno da BILHON.

---

## Quando Usar

- Novos arquivos adicionados em `/07-BILHON/00-INBOX/`
- Atualização de contexto da empresa necessária
- Processamento de calls, planilhas, documentos internos

---

## O Que Faz

1. **Escaneia** `/07-BILHON/00-INBOX/` por arquivos não processados
2. **Classifica** cada arquivo por tipo e categoria
3. **Extrai** CHUNKS e INSIGHTS relevantes
4. **Move** para pasta destino apropriada
5. **Indexa** em `_INDEX.md` e `BILHON-STATE.json`

---

## Fluxo de Processamento

```
┌────────────────────────────────────────────────────────────────────────────┐
│  PASSO 1: SCAN                                                              │
├────────────────────────────────────────────────────────────────────────────┤
│  1. Listar arquivos em /07-BILHON/00-INBOX/                                │
│  2. Ignorar README.md                                                       │
│  3. Para cada arquivo:                                                      │
│     - Detectar extensão                                                     │
│     - Ler conteúdo                                                          │
│     - Classificar tipo                                                      │
└────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────────┐
│  PASSO 2: CLASSIFICAÇÃO                                                     │
├────────────────────────────────────────────────────────────────────────────┤
│  Detectar tipo baseado em:                                                  │
│  - Nome do arquivo (call-, planilha-, contrato-)                           │
│  - Extensão (.txt, .csv, .pdf, .md)                                        │
│  - Conteúdo (keywords: "transcrição", "reunião", "KPI", etc.)              │
│                                                                             │
│  TIPOS:                                                                     │
│  - CALL_MENTOR: Calls com mentores externos                                │
│  - CALL_TEAM: Reuniões internas                                            │
│  - CALL_CLIENT: Calls com clientes                                         │
│  - FINANCE: Planilhas financeiras                                          │
│  - CONTRACT: Contratos                                                      │
│  - TEAM: Documentos de time                                                │
│  - STRATEGY: Documentos estratégicos                                       │
│  - PRODUCT: Documentação de produto                                        │
│  - MARKETING: Material de marketing                                         │
└────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────────┐
│  PASSO 3: EXTRAÇÃO (por tipo)                                               │
├────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  CALL_MENTOR:                                                               │
│  - Extrair DNA 5 camadas ([FILOSOFIA], [MODELO-MENTAL], [HEURISTICA],      │
│    [FRAMEWORK], [METODOLOGIA])                                              │
│  - Gerar INSIGHTS-[NOME-MENTOR].md em /07-STRATEGY/                        │
│  - Criar CHUNKS indexados                                                   │
│                                                                             │
│  CALL_TEAM:                                                                 │
│  - Extrair DECISÕES tomadas                                                │
│  - Extrair ACTION ITEMS com responsáveis                                   │
│  - Gerar resumo executivo                                                  │
│                                                                             │
│  FINANCE:                                                                   │
│  - Extrair métricas-chave (MRR, CAC, LTV, etc.)                           │
│  - Atualizar DAILY-SNAPSHOT.yaml                                           │
│  - Detectar thresholds críticos                                            │
│                                                                             │
│  STRATEGY:                                                                  │
│  - Indexar decisões                                                         │
│  - Vincular com contexto existente                                         │
│                                                                             │
└────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────────┐
│  PASSO 4: MOVIMENTAÇÃO                                                      │
├────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  DESTINOS:                                                                  │
│  - CALL_MENTOR → /07-BILHON/01-CALLS/MENTORS/                             │
│  - CALL_TEAM → /07-BILHON/01-CALLS/TEAM/                                  │
│  - CALL_CLIENT → /07-BILHON/01-CALLS/CLIENTS/                             │
│  - FINANCE → /07-BILHON/02-FINANCE/                                       │
│  - CONTRACT → /07-BILHON/03-CONTRACTS/                                    │
│  - TEAM → /07-BILHON/04-TEAM/                                             │
│  - MARKETING → /07-BILHON/05-MARKETING/                                   │
│  - PRODUCT → /07-BILHON/06-PRODUCTS/                                      │
│  - STRATEGY → /07-BILHON/07-STRATEGY/                                     │
│                                                                             │
│  AÇÕES:                                                                     │
│  - Renomear com prefixo [BILHON]                                           │
│  - Mover para pasta destino                                                 │
│  - Criar backup em 00-INBOX/PROCESSED/ se necessário                       │
│                                                                             │
└────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────────┐
│  PASSO 5: INDEXAÇÃO                                                         │
├────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  1. Atualizar /07-BILHON/_INDEX.md:                                        │
│     - Adicionar entrada com link                                            │
│     - Atualizar contadores                                                  │
│     - Registrar data de processamento                                       │
│                                                                             │
│  2. Atualizar /07-BILHON/BILHON-STATE.json:                               │
│     - Incrementar contadores por categoria                                  │
│     - Atualizar last_processed_at                                          │
│     - Registrar arquivo processado                                          │
│                                                                             │
│  3. Gerar LOG em /06-LOGS/BILHON-PIPELINE/:                               │
│     - PROCESS-YYYY-MM-DD-HHmm.md                                           │
│     - Detalhes de cada arquivo processado                                   │
│                                                                             │
└────────────────────────────────────────────────────────────────────────────┘
```

---

## Output

Após processamento, entregar relatório:

```
╔══════════════════════════════════════════════════════════════════════════════╗
║                    BILHON INBOX PROCESSING - COMPLETE                        ║
╠══════════════════════════════════════════════════════════════════════════════╣
║  Data: YYYY-MM-DD HH:MM                                                      ║
║  Arquivos processados: N                                                     ║
╚══════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────┐
│  ARQUIVOS PROCESSADOS                                                       │
├────────────────────────────────────┬────────────┬───────────────────────────┤
│  ARQUIVO                           │ TIPO       │ DESTINO                   │
├────────────────────────────────────┼────────────┼───────────────────────────┤
│  [arquivo1.txt]                    │ CALL_MENTOR│ /01-CALLS/MENTORS/        │
│  [arquivo2.csv]                    │ FINANCE    │ /02-FINANCE/              │
└────────────────────────────────────┴────────────┴───────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│  INSIGHTS EXTRAÍDOS                                                         │
├─────────────────────────────────────────────────────────────────────────────┤
│  [Lista de insights principais]                                             │
└─────────────────────────────────────────────────────────────────────────────┘

✅ _INDEX.md atualizado
✅ BILHON-STATE.json atualizado
✅ Log salvo em /06-LOGS/BILHON-PIPELINE/
```

---

## Regras

- **NUNCA** deletar arquivos originais sem backup
- **SEMPRE** atualizar _INDEX.md após processamento
- **SEMPRE** gerar log de processamento
- **SEMPRE** aplicar marcação [BILHON] nos arquivos movidos

---

**Versão:** 1.0.0
**Última atualização:** 2026-01-11
