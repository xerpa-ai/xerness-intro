# Xerness · FAQ

This document collects high-frequency questions about Xerness and standard external answers.
It is written for product managers, operators, and partners, and is intended to support external briefings and AMA sessions.

---

## 1. Positioning

### 1.1 What is Xerness?

Xerness is a team-level AI collaboration system for engineering organizations. It sits above AI coding tools such as Cursor, Claude Code, and Codex, and provides team-layer role specialization, workflow orchestration, repository-native memory, and cross-role handoff so that teams can use AI in a unified way.

### 1.2 How does it relate to Cursor or Claude Code?

They are not competitors. Cursor and Claude Code are AI tools used by individual developers (the *individual layer*). Xerness is the *team layer* built on top of them — analogous to the relationship between personal productivity apps and an enterprise collaboration system.

### 1.3 How is it different from Coze, Dify, or similar platforms?

Coze and Dify are no-code platforms for building **business-side** AI applications (customer support bots, marketing automation, business workflows). Xerness is built for **internal engineering team collaboration** and serves the software development process itself. They occupy different categories.

### 1.4 How is it different from GitHub Copilot?

Copilot optimizes the experience of a single developer. Xerness optimizes how the whole team uses AI tools — including Copilot — together.

---

## 2. Value

### 2.1 What concretely changes after a team adopts Xerness?

| Dimension | Before | After |
|-----------|--------|-------|
| Prompts and workflows | Each engineer figures it out alone | Team-wide standard |
| Knowledge reuse | The same mistakes repeat | Lessons captured automatically; future agents avoid them |
| Cross-role handoff | Copy-paste from chat | Automatic artifact passing |
| New-hire onboarding | Long ramp-up | Day-one access to the full team operating model |
| Team efficiency | Individuals improve, team does not | Team-level efficiency improves alongside individuals |

### 2.2 How is ROI measured?

Internal observations to date:

- New-hire time-to-first-independent-output drops from roughly two weeks to about three days
- Code review consistency increases significantly; outcomes depend less on which reviewer is assigned
- Recurrence of historical issues and known pitfalls drops noticeably

Specific gains depend on team size and current state. We are happy to discuss in detail during AMA.

### 2.3 How does it differ from Confluence, Notion, and similar knowledge bases?

Content in Confluence or Notion is **read by humans**, and adoption typically decays over time. Content in Xerness is **read directly by AI** and is automatically applied during execution — it does not depend on people remembering to look it up.

---

## 3. Usage

### 3.1 Do we need to replace our existing AI tools?

No. Cursor, Claude Code, and Codex remain in place. Xerness sits above them as a collaboration layer; it does not replace the tools themselves.

### 3.2 How long does adoption take?

A typical project takes 1–2 working days:

- Day 1: install, integrate the repository, and run the basic flow end-to-end
- Day 2: align role definitions and workflows, then trial-run 1–2 real tasks

### 3.3 What team size is the sweet spot?

Best fit: 3–30 people.

- Smaller teams: collaboration benefits are limited
- Larger teams: roll out by sub-team

### 3.4 Do non-technical members (Product / Operations) need to learn anything?

Effectively no. Product managers submit requests as usual; the system handles handoff to engineering and QA. Operations typically do not interact with Xerness directly but benefit from faster and more reliable engineering delivery.

---

## 4. Commercial and Open Source

### 4.1 Will Xerness be open source?

Yes. The boundary between open source and commercial editions:

| Scope | Contents |
|-------|----------|
| Open source | Core engine, collaboration protocols, base workflow templates, public capability packs |
| Commercial | Org-level governance, rollout controls, audit and permissions, private skill packs, enterprise onboarding |

Design principle: **open source drives adoption; the commercial layer makes adoption safe at scale.**

### 4.2 How is the commercial edition priced?

Tiered by team / company size. Pricing will be announced at the commercial release. The open-source edition will remain free permanently.

### 4.3 How is data and code security handled?

Xerness itself does not store user code or business data:

- Workflows, standards, and memory live in the team's own Git repository
- LLM calls use the team's own API credentials
- Xerness does not access user source code or business data

---

## 5. Product Direction

### 5.1 Which AI tools are supported?

Current priorities:

| Priority | Tool | Rationale |
|----------|------|-----------|
| P0 | Cursor / Claude Code | Fastest path to team adoption; strongest validation surface |
| P1 | Codex | Strengthens end-to-end engineering execution |
| P2 | OpenClaw | Expands into the persistent-assistant / SOUL ecosystem |

### 5.2 What is the underlying thesis?

1. AI has materially improved individual developer productivity (already happened)
2. Team-level productivity does not automatically follow (current reality)
3. A team-layer AI collaboration system will inevitably emerge (necessary)
4. Establishing the standard early creates a first-mover advantage (opportunity)

---

## 6. Technical Questions

> The following questions target technically-inclined readers. Full details are in [ARCHITECTURE.md](./ARCHITECTURE.md).

### 6.1 What model does Xerness use?

By default, the Anthropic Claude Agent SDK (`claude-sonnet-4.6`), with OpenRouter compatibility for other models. Configurable per team via `xerness.config.yaml`.

### 6.2 How does "agents automatically follow team standards" actually work?

Through two mechanisms:
1. **Standards** skills inject relevant rules into the agent's system prompt at every call.
2. **Memory** is retrieved before execution so historical decisions and lessons are surfaced into context.

Together, these constrain agent behavior without relying on humans to remind it.

### 6.3 How are workflows defined?

Declaratively, as YAML DAGs. Each node corresponds to one role agent; its output flows automatically to the next node. The DAGScheduler decides what runs in parallel and what runs in series.

### 6.4 What are the prerequisites for adoption?

- Node.js 20+
- The team's own Anthropic / OpenRouter API key
- A target repository (Cursor / Claude Code / Codex compatible)

### 6.5 Is there a server component to deploy?

**No.** Xerness ships as an npm package. All configuration and memory live in the team's own Git repository; LLM calls use the team's own credentials.

### 6.6 What is the relationship to the Claude Agent SDK?

The Claude Agent SDK is one of the **underlying invocation interfaces** Xerness uses. Xerness builds the team-layer protocols, workflow orchestration, and memory system on top of it. They operate at different layers.

---

## 7. Common Objections

### 7.1 Isn't this just "prompt engineering as a project"?

No. Prompts are only the surface. The substance of Xerness includes:

- Role-level protocols
- Cross-role handoff protocols
- Repository-native structured knowledge capture
- Composable AI collaboration mechanisms within the engineering process

Prompts are a small part of the whole.

### 7.2 Won't a middle layer be obsoleted by faster-improving foundation models?

Stronger models do not eliminate the question of *how a team coordinates around them*. In fact, as models improve, the gap between teams **with** a collaboration layer and teams **without** one grows wider, not narrower.

### 7.3 Why not just build this on top of Coze or Dify?

Coze and Dify are no-code **business-process** automation platforms. Their target scenarios do not overlap with Xerness. Using a business-process tool to manage how an engineering team works is a mismatch of abstraction.

---

## 8. Standard External Talking Point

If you have 30 seconds to introduce Xerness, the recommended phrasing is:

> Xerness is a team-level AI collaboration system.
>
> Engineering teams already use AI, but usage is fragmented, lessons are not captured, and cross-role collaboration depends on manual copy-paste.
>
> Xerness adds a team operating layer on top of Cursor, Claude Code, and similar tools — turning team roles, standards, memory, and handoff into assets that AI agents automatically apply.
>
> In one line: **it solves not "how an individual uses AI", but "how a team uses AI together".**

---

*If your question is not covered here, open an [issue](https://github.com/xerpa-ai/xerness-intro/issues) or reach us via [xagt.ai](https://xagt.ai) / [@XAgent_official](https://x.com/XAgent_official).*

---

*Part of the [XAgent](https://xagt.ai) family.*
