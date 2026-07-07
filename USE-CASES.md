# Xerness · Use Cases

This document presents three concrete scenarios showing how a team's working style changes before and after adopting Xerness. The scenarios are written in non-technical language and are appropriate for product managers, operators, and external partners.

---

## Scenario 1 · From Request to Production

**Context:** A product manager requests a new feature — *"users should be able to view their order history after logging in."*

### Before Xerness

```
Product (in chat tool) ── describes the request
       │
       ▼  manually pasted into the IDE
Engineering ── interprets, designs, and implements
              (significant time spent inferring intent)
       │
       ▼  notifies QA when "done"
QA ── re-reads the original request, designs test cases from scratch
       │
       ▼
Release ── no unified change record; problems are mitigated by rollback
```

Each step contains repeated work and risk of misinterpretation.

### After Xerness

```
Product ── submits the request
       │
       ▼  Product agent automatically produces a PRD
       │
       ▼  Engineering agent picks up the PRD and produces design + code
       │
       ▼  QA agent automatically generates test cases
       │
       ▼  DevOps agent automatically generates a release checklist
       │
       ▼
Production
```

**Key difference:** the output of each step is automatically passed to the next. Cross-role handoff no longer depends on manual transfer of context.

---

## Scenario 2 · Onboarding a New Engineer

**Context:** A new engineer joins a project that has been iterating for six months.

### Before Xerness

| Phase | Behavior |
|-------|----------|
| Days 1–3 | Reads code; many decisions are opaque |
| Days 4–7 | Repeatedly checks with senior engineers about background and historical decisions |
| Days 8–14 | Begins independent output, but still risks repeating known pitfalls |

Typical time to independent output: **about two weeks.**

### After Xerness

From day one, the new engineer's AI already has access to:

- The full background of historical decisions on the project
- The team's coding standards and review criteria
- Pitfalls already encountered, along with the agreed mitigations
- The current iteration's goals and outstanding work

While coding, the AI proactively surfaces relevant history:

> *"A similar implementation existed in this area; it was later replaced with pattern Y because of issue X."*

Typical time to independent output: **about three days.**

---

## Scenario 3 · Coordinating Across Different AI Tools

**Context:** Team members use different AI coding tools — some Cursor, some Claude Code, some experimenting with Codex.

### Before Xerness

Each engineer maintains private prompt templates and habits, leading to:

- Code style differences across team members
- The same request implemented very differently by different engineers
- Code review outcomes that depend on which reviewer is assigned

Team-level efficiency is roughly the sum of individual efficiencies. **There is no compounding.**

### After Xerness

Regardless of which tool each engineer uses, the **shared standards, workflows, and memory** provided by Xerness apply consistently:

```
       ┌── Cursor (engineer A) ──┐
       │                         │
shared │                         │
rules  ├── Claude Code (B) ──────┤── style and quality converge
       │                         │
       └── Codex (engineer C) ───┘
```

Different tools, **convergent output.** Predictability of team output increases substantially.

---

## Prerequisites

The three scenarios assume the following:

- Team size of at least three engineers (smaller teams see limited benefit)
- The team is already using AI coding tools (Xerness does not bootstrap AI adoption)
- An ongoing iterating project (not a one-off script)

---

## Going deeper

- **Get started** — see the [Quickstart](./README.md#quickstart) in the README
- **How it works internally** — see [ARCHITECTURE.md](./ARCHITECTURE.md)
- **Early access or a live walkthrough** — reach us via [xagt.ai](https://xagt.ai) / [@XAgent_official](https://x.com/XAgent_official)

---

*Part of the [XAgent](https://xagt.ai) family. Additional scenarios will be added as the product evolves.*
