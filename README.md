# write-project-docs

A Claude Code plugin that systematically generates complete technical documentation for mature software projects.

## Features

- **Multi-project type support** — ML, backend services, frontend, SDK, CLI, Go, C/C++, Rust, DevOps, mobile apps
- **Five-phase workflow** — Discovery → Structure → Generation → Validation → Polish
- **Code-consistent validation** — Two-agent validation system catches documentation-code mismatches
- **Type-specific templates** — Tailored documentation structures for each project type
- **Mermaid diagrams** — Visual documentation with flowcharts, sequence diagrams, class diagrams

## Installation

### Via Claude Code CLI

```bash
claude plugin add wqvbjhc/write-project-docs
```

### Manual

Clone this repository and ensure the `.claude-plugin/` directory is recognized by your Claude Code environment.

## Usage

Invoke the skill in Claude Code:

```
/write-project-docs
```

The skill triggers automatically when you say things like:

- "写文档" / "生成技术文档" / "补充项目文档"
- "写 README" / "文档交接" / "项目文档化"
- "帮我整理下这个项目"
- "新人要接手，准备下资料"

## Workflow

```
Discovery → Structure → Generation → Validation → Polish
   ↑                                      |
   └──────── User Feedback Loop ←─────────┘
```

1. **Discovery** — Project scanning, type identification, user interview
2. **Structure** — Documentation architecture design based on project type
3. **Generation** — Content generation with code-verified facts
4. **Validation** — Two parallel agents verify consistency and completeness
5. **Polish** — Fix issues, unify formatting, finalize

## Project Structure

```
.claude-plugin/
├── plugin.json            # Plugin metadata
└── marketplace.json       # Remote installation config
skills/
└── write-project-docs/
    └── SKILL.md           # Main skill methodology
```

## License

MIT
