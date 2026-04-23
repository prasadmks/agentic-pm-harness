# Agentic PM Harness

A chain of five Claude Code skills that runs the full PM discovery workflow — from company context to clickable prototype — end to end.

Each skill runs independently or chains sequentially. State persists across steps through structured output files: company context flows into market research, research informs the PRD, PRD drives the prototype.

---

## The Pipeline

```
Company Context → Market Research → User Research → PRD → Prototype
```

| Skill | File | What it produces |
|---|---|---|
| Company Context | `skills/company-context-builder.md` | 4 structured context files: overview, persona, product, competitive landscape |
| Market Research | `skills/market-research.md` | Market sizing, trends, competitive dynamics, strategic implications |
| User Research | `skills/user-research.md` | JTBD, key findings, pain points, product implications |
| PRD | `skills/prd.md` | Full PRD with P0/P1/P2 requirements, success metrics, open questions |
| Prototype | `skills/mock.md` | Clickable single-file HTML prototype built from P0 requirements |

---

## How To Use

### Prerequisites
- [Claude Code](https://claude.ai/code) installed
- Claude Pro subscription (required for skill chaining and `/loop`)

### Setup
1. Clone this repo
2. Open the folder in Claude Code
3. Drop the skills into your `.claude/skills/` directory

### Run the full pipeline
```
/company-context-builder
```
Answer the questions. Four context files are saved to `company-context/`.

Then run each skill in sequence — or run them independently for standalone use:
```
/market-research [topic]
/user-research [segment]
/prd [feature or product name]
/mock
```

The mock skill reads your `company-context/` folder and your PRD automatically — no manual input required.

---

## Example

A worked example showing what the harness produces end to end:

### Charter Communications — Spectrum Aura
Full pipeline output for Charter's agentic AI customer experience platform — the production system serving 32M+ customers across 250M annual contacts.

→ [`examples/charter-spectrum/`](https://github.com/prasadmks/agentic-pm-harness/tree/main/examples/charter-spectrum)

Includes:
- `company-overview.md` — company snapshot, mission, business model, key facts
- `user-persona.md` — Marcus Webb composite persona, goals, pain points, day in the life
- `product-description.md` — Aura platform, Know/Act/Trust architecture, use cases, integrations
- `competitive-landscape.md` — Comcast, AT&T, T-Mobile, ASAPP, Google CCAI comparison

---

## Repo Structure

```
agentic-pm-harness/
├── README.md
├── skills/
│   ├── company-context-builder.md
│   ├── market-research.md
│   ├── user-research.md
│   ├── prd.md
│   └── mock.md
└── examples/
    └── charter-spectrum/
        ├── company-overview.md
        ├── user-persona.md
        ├── product-description.md
        └── competitive-landscape.md
```

---

## Built by
Prasad MK · [linkedin.com/in/prasadmk](https://linkedin.com/in/prasadmk) · prasad.mks@gmail.com
