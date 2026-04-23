# Agentic PM Harness

A full PM discovery pipeline — from company context to clickable prototype — built as a chain of Claude Code skills where each step produces structured artifacts that feed the next.

---

## The Problem

Discovery work follows the same structure on every product bet — company context, market research, user research, PRD, prototype. Most PMs rebuild it from scratch each time, across disconnected tools, losing context at every handoff. By the time a prototype exists, the research that shaped it is buried in a different document or a different conversation.

This harness changes the architecture: each skill produces a structured artifact on disk, and the next skill reads it automatically. The research informs the PRD. The PRD drives the prototype. State doesn't get lost — it accumulates.

---

## How It Works

![Agentic PM Harness Architecture](https://raw.githubusercontent.com/prasadmks/agentic-pm-harness/main/images/agentic-pm-harness-architecture.png)

Each skill runs independently or chains sequentially. State persists across steps — the Prototype skill reads the PRD and company context automatically, no manual input required.

---

## The Design Decision

Most AI-assisted workflows produce text in a chat window — no memory, each session starts from scratch. This harness is built differently: every skill writes structured output to disk, and the next skill reads it.

| Characteristics | Prompt-based workflow | Agentic Harness |
|---|---|---|
| State | Lost after each session | Persists on disk across steps |
| Output | Text in chat | Structured markdown artifacts |
| Reusability | One-time | Runs on any company, any product |
| Handoff | Manual copy-paste | Automatic — each skill reads the last |

---

## What's In This Repo

```
agentic-pm-harness/
├── README.md
├── skills/
│   ├── company-context-builder.md   ← scrapes website, generates 4 context files
│   ├── market-research.md           ← market sizing, trends, competitive dynamics
│   ├── user-research.md             ← JTBD, findings, pain points, implications
│   ├── prd.md                       ← full PRD with P0/P1/P2 requirements
│   └── mock.md                      ← clickable HTML prototype from P0 requirements
├── examples/
│   └── charter-spectrum/
│       ├── company-overview.md      ← Spectrum Aura platform context
│       ├── user-persona.md          ← composite persona, goals, pain points
│       ├── product-description.md   ← Know/Act/Trust architecture, use cases
│       └── competitive-landscape.md ← Comcast, AT&T, T-Mobile, ASAPP comparison
└── images/
    └── agentic-pm-harness-architecture.png
```

The `examples/charter-spectrum/` folder shows full pipeline output for Spectrum Aura — Charter's agentic AI platform serving 32M+ customers across 250M annual contacts.

---

## Built by
Prasad MK · [linkedin.com/in/prasadmk](https://linkedin.com/in/prasadmk) · prasad.mks@gmail.com
