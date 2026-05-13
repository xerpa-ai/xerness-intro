# Xerness · Architecture

This document describes the system layers, key components, typical workflow, and technology choices of Xerness.
It is intended for partners and technical liaisons who need a deeper understanding of how Xerness works internally. **It does not include source code or internal prompt content.**

---

## 1. System Layers

Xerness is organized into four layers, top to bottom:

```
┌─────────────────────────────────────────────────────────┐
│  Adapter Layer                                          │
│  CLI · Cursor · Claude Code · Codex · OpenClaw          │
├─────────────────────────────────────────────────────────┤
│  Orchestration Layer                                    │
│  IntentParser · DAGScheduler · WorkflowEngine           │
├─────────────────────────────────────────────────────────┤
│  Execution Layer                                        │
│  AgentLoop · BaseAgent · SkillRegistry · SkillLoader    │
├─────────────────────────────────────────────────────────┤
│  Infrastructure Layer                                   │
│  AgentClient · MessageBus · ArtifactStore · MemoryStore │
│  Logger                                                 │
└─────────────────────────────────────────────────────────┘
```

Responsibilities:

| Layer | Responsibility |
|-------|----------------|
| Adapter | Normalizes different IDEs / agent surfaces to a single internal protocol; provides the CLI entry point |
| Orchestration | Parses intent, builds the DAG, schedules execution |
| Execution | Per-role agent execution loop and capability extension |
| Infrastructure | LLM invocation, message bus, artifact store, memory access, logging |

---

## 2. Key Components

### 2.1 IntentParser

Parses a natural-language request into a structured `ParsedIntent`.

- Reads the most recent 20 git commits as context
- Identifies which role the request belongs to (PM / Tech / Test / DevOps)
- Extracts key entities and goals
- Outputs a structure consumable by the DAGScheduler

### 2.2 DAGScheduler

Schedules agent nodes based on the dependency graph defined in a workflow.

- **Input:** workflow YAML (declarative DAG)
- **Output:** the artifact produced by each node
- **Capabilities:** parallel / serial scheduling, retry on failure, automatic artifact passing

### 2.3 BaseAgent / AgentLoop

The base class and execution loop for each role agent:

- Loads the role's SOUL (behavior definition)
- Injects relevant skills and memory
- Calls the underlying LLM SDK via `AgentClient`
- Emits a standardized artifact for the next node

### 2.4 SkillRegistry / SkillLoader

The skill system has two kinds of skills:

| Type | Role |
|------|------|
| **Standards** | Injected into the system prompt to constrain agent behavior (coding standards, review checklists, etc.) |
| **Capabilities** | Registered as `tool_use` so agents can call them directly (`shell-exec`, `http-fetch`, etc.) |

Each skill is a directory containing a `SKILL.md` file (YAML frontmatter + content). Skills can be forked, extended, and reused.

### 2.5 MemoryStore

A repository-native, structured memory layer:

```
.agent/memory/
├── decisions/   recorded decisions
├── lessons/     captured pitfalls
├── patterns/    reusable patterns
├── solutions/   prior solutions
└── context/     project background
```

Each memory entry is a markdown file with frontmatter, version-controlled alongside the project.

### 2.6 Routing

Xerness implements prompt-level routing through `routing.yaml`:

- **Cursor:** continuous routing via `.cursor/rules/*.mdc scope=always`
- **Claude Code:** per-turn routing via the `UserPromptSubmit` hook

When the user submits a prompt in the IDE, the hook classifies it (L0 / L1 / L2) and injects the corresponding role's SOUL and required skills.

---

## 3. A Typical Workflow

End-to-end flow for *"product manager submits a request → production":*

```
1. User input: "Build a user order-history page"
                    │
                    ▼
2. IntentParser
   · reads recent git log
   · identifies the entry role (PM)
   · emits ParsedIntent
                    │
                    ▼
3. DAGScheduler loads workflow.yaml
   · Node 1: PM Agent       → produces a PRD
   · Node 2: Tech Agent     → produces design + code
   · Node 3: Test Agent     → produces test cases
   · Node 4: DevOps Agent   → produces a release checklist
                    │
                    ▼
4. AgentLoop executes each node in order
   · each agent loads its own SOUL + relevant skills + memory
   · invokes the Claude Agent SDK
   · writes the resulting artifact to the ArtifactStore
                    │
                    ▼
5. The full pipeline output lives inside the project repository
   · PRD.md / impl/ / tests/ / release-checklist.md
   · key decisions are written to .agent/memory/decisions/
```

---

## 4. Technology Choices

| Dimension | Choice | Rationale |
|-----------|--------|-----------|
| Language | TypeScript (strict mode) | Type safety; fits mainstream IDE / Node ecosystems |
| Runtime | Node.js 20+, ESM | Modern JS ecosystem default |
| Package manager | pnpm workspace | Standard for multi-package repositories |
| Testing | Vitest | Fast and ESM-friendly |
| Default model | `anthropic/claude-sonnet-4.6` (via OpenRouter) | Best fit for engineering collaboration today; OpenRouter simplifies multi-model swap |
| Configuration | YAML | Declarative; readable and editable by non-engineering roles |

---

## 5. Extensibility

Xerness is designed to be **rewritten by the adopting team**:

- **Roles** — drop a new role directory under `.agent/agents/`
- **Skills** — duplicate the SKILL.md template and add a new skill directory
- **Workflows** — edit YAML directly; no core code changes required
- **Routing** — `routing.yaml` controls which prompts go to which role

---

## 6. Out of Scope for This Repository

To protect commercial assets and team know-how, the following are not included here:

- The actual content of internal SOUL.md files
- Implementation of governance, audit, and org-level rollout in the commercial edition
- Implementation of private skill packs
- Internal performance / cost data

For deeper access, please contact XerpaAI through business channels.

---

*This architecture document is updated as the product evolves.*
