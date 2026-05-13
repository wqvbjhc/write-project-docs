# write-project-docs

> AI-powered technical documentation that never lies — every claim verified against your actual code.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://github.com/wqvbjhc/write-project-docs)

---

## The Problem

You have a mature codebase with 50k+ lines of code. The README is outdated, there's no architecture doc, and a new teammate is joining next week.

You ask AI to "write docs" and get:
- Parameters that **don't exist** in your code
- Commands that **don't actually work**
- Architecture diagrams that **contradict** your actual data flow
- Generic FAQ answers copied from **Stack Overflow templates**

**Wrong documentation is worse than no documentation.**

## The Solution

This Claude Code skill generates documentation where **every fact is grep-verified against your actual code** — parameter names, default values, API routes, version numbers. Nothing is written from memory.

```
You: "write docs for this project"

write-project-docs:
  → Scans your codebase and identifies project type (Go backend? ML pipeline? Rust CLI?)
  → Interviews you about design decisions and target audience
  → Proposes doc structure, waits for your approval
  → Generates content, every claim backed by code grep
  → Runs TWO validation agents in parallel to catch inconsistencies
  → Shows you what they found, fixes issues, delivers final docs
```

## Example Output

Given a Go backend service, the skill produces:

```
docs/
├── README.md              # Project overview, quick start, tech stack
├── quick-start.md         # Environment → Start server → Call API → Check logs
├── api-reference.md       # Every endpoint: method, path, params, response, errors
├── architecture.md        # Layered design, module interactions, data models, auth flow
├── deployment.md          # Containerization, CI/CD, monitoring, scaling, DB migration
└── faq.md                 # Real problems from your project, not generic answers
```

Every parameter in `api-reference.md` is extracted from your route registration code.
Every config value in `deployment.md` is verified against your actual Dockerfile and compose files.

## Before & After

<table>
<tr><th>Without this skill ❌</th><th>With this skill ✅</th></tr>
<tr><td>

```markdown
## API

POST /api/users - Create user
GET /api/user/:id - Get user
```
*(route is actually `/api/v2/users`)*
*(param is `userID`, not `id`)*

</td><td>

```markdown
## API

POST /api/v2/users - Create user
GET /api/v2/users/:userID - Get user
  Response: { "id": int, "name": string }
  Errors: 404 UserNotFound, 422 ValidationError
```
*(every path grep-verified from router.go)*

</td></tr>
<tr><td>

```markdown
## Quick Start

Run `python train.py --model resnet`
```
*(flag is actually `--arch`, not `--model`)*

</td><td>

```markdown
## Quick Start

CUDA_VISIBLE_DEVICES="0" python train.py \
    --arch resnet50 \
    --data-path /path/to/imagenet \
    --lr 0.1
```
*(args extracted from argparse in train.py:L42)*

</td></tr>
</table>

## Supported Project Types

| Type | What Gets Documented |
|------|---------------------|
| **ML / Data Science** | Training args, data pipeline, model architecture, deployment |
| **Backend Service** | API reference, architecture, auth flow, deployment & ops |
| **Frontend App** | Components, state management, routing, build & deploy |
| **SDK / Library** | API reference, migration guide, contributing guide |
| **CLI Tool** | Command reference, config, plugin development |
| **Go / Rust / C/C++** | Public API, build system, architecture, platform porting |
| **DevOps / Infra** | Topology, runbooks, alerting, incident response |
| **Monorepo** | Package index, dependency graph, shared tooling |
| **Data Engineering** | Pipeline catalog, data lineage, schema reference |

## Installation

```bash
claude install-skill wqvbjhc/write-project-docs
```

Or clone manually:

```bash
git clone https://github.com/wqvbjhc/write-project-docs.git
```

## Usage

Just say what you need in Claude Code:

```
write docs for this project
document this codebase
generate README and API reference
帮我整理下这个项目的文档
新人要接手，准备下资料
```

Or invoke directly:

```
/write-project-docs
```

## How It Works

```
Discovery → Structure → Generation → Validation → Polish
   ↑                                      |
   └──────── User Feedback Loop ←─────────┘
```

| Phase | What Happens | You Approve Before Next Phase |
|-------|-------------|------|
| **Discovery** | Scans codebase, identifies project type, interviews you | ✅ |
| **Structure** | Designs doc architecture for your project type and scale | ✅ |
| **Generation** | Writes content — every fact backed by `grep` | ✅ |
| **Validation** | 2 agents in parallel: one checks code consistency, one checks cross-references | ✅ |
| **Polish** | Fixes all issues, unifies formatting, commits | ✅ |

**Key guarantee**: Nothing proceeds without your confirmation. You stay in control.

## What Makes This Different

| Feature | Most AI doc tools | write-project-docs |
|---------|-------------------|-------------------|
| Fact verification | Trust the model's memory | Every fact grep-checked against code |
| Validation | None or single-pass | Two independent agents in parallel |
| User control | Generate and dump | Checkpoint after every phase |
| Project awareness | One-size-fits-all | 13 type-specific templates and validation rules |
| Existing docs | Ignores them | Detects, audits, decides update vs rewrite |
| Doc frameworks | Markdown only | Adapts to MkDocs, Docusaurus, Sphinx, mdBook |

## License

MIT
