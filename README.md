# Xerness

**The team operating layer for AI coding agents.**

Xerness is a team-level collaboration system for engineering organizations. It sits above AI coding tools such as Cursor, Claude Code, Codex, and OpenClaw, and adds the missing team layer — role specialization, workflow orchestration, repository-native memory, and cross-role handoff — turning *individual use of AI* into *coordinated team use of AI*.

> This repository is a **public overview** of Xerness, written for product managers, operators, partners, and technical liaisons.
> It contains no source code or internal implementation details.
>
> Maintainer: **XerpaAI** · Status: in active internal use, preparing core-layer open source release.

---

## Table of Contents

- [1. Background and Positioning](#1-background-and-positioning)
- [2. Product Architecture](#2-product-architecture)
- [3. Core Capabilities](#3-core-capabilities)
- [4. Technical Architecture](#4-technical-architecture)
- [5. Relationship to Existing Tools](#5-relationship-to-existing-tools)
- [6. Who It Is For](#6-who-it-is-for)
- [7. Current Status](#7-current-status)
- [8. Further Reading](#8-further-reading)
- [9. Contact](#9-contact)

---

## 1. Background and Positioning

Over the past two years, AI coding tools (Cursor, Claude Code, Copilot, Codex, etc.) have meaningfully improved **individual** developer productivity. At the team level, however, we observe a consistent set of unresolved problems:

| Dimension | Current Reality |
|-----------|-----------------|
| Usage style | Each engineer invents their own prompts and workflows; no shared standard |
| Knowledge retention | Lessons learned are scattered across chat history; rarely reused |
| Cross-role handoff | Product → Engineering → QA → Release relies on manual copy-paste |
| New-hire onboarding | No structured way to transfer "how this team uses AI" |
| Team-level efficiency | Individuals are 30% faster, but the team is not |

Xerness is positioned to fill that team operating layer:

> Existing AI tools answer *whether AI can do the work*.
> Xerness answers *how a team uses AI together*.

---

## 2. Product Architecture

```
┌──────────────────────────────────────────────────────────┐
│         Team (Product / Engineering / QA / DevOps)       │
└──────────────────────────────────────────────────────────┘
                            │
              ┌─────────────▼─────────────┐
              │          Xerness          │
              │   Team operating layer    │
              │                           │
              │  · Role routing           │
              │  · Workflows & handoffs   │
              │  · Repo-native memory     │
              │  · Review & ship rituals  │
              └─────────────┬─────────────┘
                            │
        ┌──────────┬────────┼─────────┬──────────┐
        ▼          ▼        ▼         ▼          ▼
     Cursor   Claude Code  Codex   OpenClaw     ...

                AI coding & execution tools
```

---

## 3. Core Capabilities

### 3.1 Role Specialization

Xerness does not provide a single general-purpose assistant. Instead, it defines distinct engineering roles. Each role has its own behavior definition (SOUL), workflow preferences, and skill set.

| Role | Responsibility |
|------|----------------|
| Product (PM) | Vague request → PRD |
| Engineering (Tech) | Technical design and implementation |
| QA (Test) | Test cases and regression |
| DevOps | Deployment and monitoring |

### 3.2 Workflow Orchestration

Tasks are described declaratively as YAML DAGs. The scheduler decides which steps run in parallel and which must run in series. Each node corresponds to one role agent's execution; its artifact is automatically passed to the next node as input.

### 3.3 Repository-Native Memory

Team standards and lessons live **inside the project repository**, not in an external knowledge base. Memory is structured by category — decisions, lessons, patterns, solutions, context — and is retrieved by agents before execution. New team members start with the team's full operating history, not a blank slate.

### 3.4 Cross-Role Handoff

When a role completes its task, output is delivered as a standardized artifact rather than as conversational context. This makes the entire Product → Engineering → QA → Release pipeline traceable and replayable.

---

## 4. Technical Architecture

### 4.1 System Layers

```
┌─────────────────────────────────────────────────┐
│  Adapter Layer                                  │
│  CLI · Cursor · Claude Code · Codex · OpenClaw  │
├─────────────────────────────────────────────────┤
│  Orchestration Layer                            │
│  IntentParser · DAGScheduler · WorkflowEngine   │
├─────────────────────────────────────────────────┤
│  Execution Layer                                │
│  AgentLoop · BaseAgent · SkillRegistry          │
├─────────────────────────────────────────────────┤
│  Infrastructure Layer                           │
│  AgentClient · MessageBus · ArtifactStore       │
│  MemoryStore · Logger                           │
└─────────────────────────────────────────────────┘
```

### 4.2 Key Components

| Component | Responsibility |
|-----------|----------------|
| **IntentParser** | Parses natural-language requests into a structured `ParsedIntent`; injects recent git context |
| **DAGScheduler** | Schedules agent nodes per workflow definition (parallel / serial, retry, artifact passing) |
| **AgentLoop** | Per-role execution loop; wraps the underlying LLM SDK conversation flow |
| **SkillRegistry** | Registers, loads, and discovers skills (Standards and Capabilities) |
| **MemoryStore** | Repository-native structured memory access |


> See [ARCHITECTURE.md](./ARCHITECTURE.md) for the full architecture and component breakdown.

---

## 5. Relationship to Existing Tools

| Tool | Strength | What Xerness Adds |
|------|----------|-------------------|
| Cursor / Claude Code | Strong individual coding UX | Shared standards, workflows, and memory across the team |
| Codex | Strong execution on engineering tasks | Team-level coordination and handoff |
| OpenClaw | Persistent assistants and SOUL ecosystem | Repo-native workflows and release rituals |

**Xerness does not replace these tools. It adds a team layer on top of them.**

---

## 6. Who It Is For

**Suitable**

- Engineering teams of 3–30 people
- Teams already using Cursor / Claude Code / Codex
- Founders and engineering leaders who want a unified team-level AI operating model

**Not yet suitable**

- Pure individual projects or one-off scripts
- Teams that have not yet adopted AI coding tools (the "should we use AI" question must be settled first)

---

## 7. Current Status

| Module | Status |
|--------|--------|
| Core engine (IntentParser + DAGScheduler + AgentLoop) | Done |
| Role system (PM / Tech / Test / DevOps) | Done |
| Daily internal use | In production |
| Cursor integration | Done |
| Claude Code integration | Done |
| Codex integration | In progress |
| OpenClaw integration | Planned |
| Core-layer open source | In preparation |

---

## 8. Further Reading

- [ARCHITECTURE.md](./ARCHITECTURE.md) — System layers, key components, typical workflow
- [FAQ.md](./FAQ.md) — Frequently asked questions and standard external talking points
- [USE-CASES.md](./USE-CASES.md) — Three concrete before/after scenarios

> Full source code and internal implementation details remain in private repositories. Please contact us through the channels below for deeper access.

---

## 9. Contact

- **Maintainer:** XerpaAI
- **Business and partnerships:** ZIHAO

---

*Xerness is designed and maintained by the XerpaAI team. This repository is updated as the product evolves.*
