---
name: write-project-docs
description: >
  Use when the user asks to write, generate, or update technical documentation
  for an existing project. Triggers include: "write docs", "generate README",
  "document this codebase", "create API reference", "project handoff docs",
  "onboarding documentation", "写文档", "生成技术文档", "补充项目文档",
  "写 README", "文档交接", "项目文档化", "为代码写说明",
  "帮我整理下这个项目", "新人要接手，准备下资料".
  Also use when someone says "we need proper docs" or
  "organize this project for a new teammate".
---

# Technical Documentation Methodology for Mature Projects

Systematic approach for writing complete technical documentation for codebases of any type — ML, backend, frontend, SDK, CLI, Go, C/C++, Rust, DevOps, mobile, monorepo, data engineering, and more.

## Core Design Goal

**Catch documentation-code inconsistencies before submission.**

The worst failure is not bad writing — it's documentation that contradicts the code. A wrong default value, a stale command, an incomplete architecture diagram — these are more dangerous than having no docs at all.

---

## Five-Phase Workflow

```
Discovery → Structure → Generation → Validation → Polish
   ↑                                      |
   └──────── User Feedback Loop ←─────────┘
```

---

## Phase 1: Discovery

Goal: Build comprehensive understanding. Determine doc scope and project type.

### 1.1 Code Structure Scan
- Directory tree: core modules, entry scripts, config files
- `git log --oneline -20` for recent activity
- Estimate code scale (file count, line count)

### 1.2 Existing Docs Audit
- README.md, `docs/`, wiki links
- Code comment density (`@doc`, `///`, docstrings)
- CHANGELOG, CONTRIBUTING, ADRs
- CI doc generation steps (rustdoc, javadoc, Sphinx, Storybook)

### 1.3 Project Type Identification
Read `refs/project-types.md` → Type Identification table. A project may be hybrid — use primary type, supplement with secondary.

### 1.4 Entry Point Exploration (2-3 Explore Agents in Parallel)
Read `refs/project-types.md` → Exploration Directions table. Launch agents per the type-specific directions.

### 1.5 User Interview (THE most critical step)
Confirm with user:
- Recommended usage patterns (don't guess from code)
- Historical decision reasons (why A not B)
- Known pitfalls
- Target audience (new team member / external user / self-reference)
- Product/business context (where deployed, who it serves)
- **Documentation language**: English or Chinese? (Internal projects often Chinese; open-source/international projects use English)

### 1.6 Docs-as-Code Framework Detection
Check for existing doc frameworks:
- `mkdocs.yml` → MkDocs
- `docusaurus.config.js` → Docusaurus
- `conf.py` → Sphinx
- `book.toml` → mdBook (Rust)
- `hugo.toml`/`config.toml` → Hugo

If found: generate content compatible with that framework's structure, navigation config, and frontmatter format.

### 1.7 Dependency Scan
```bash
# Python: grep -rh "^import \|^from " *.py | sort -u
# Node:   grep -rh "require(\|from '" src/ | sort -u
# Go:     grep -rh "\".*\"" go.mod
# C/C++:  grep -rh "#include" src/ include/ | sort -u
# Rust:   grep -rh "^use \|extern crate" src/ | sort -u
```

### Phase 1 Deliverables
- Project type determination
- Existing doc asset inventory (reusable / needs update / needs creation)
- Module responsibility list
- Key decisions list (with reasons)
- Target audience profile
- Documentation language decision

### Decision Gate: Update vs Create
- If existing docs cover >70% of needed content and are mostly accurate → **Update Mode**: diff existing docs against code, patch inaccuracies, fill gaps
- If existing docs are sparse or severely outdated → **Rewrite Mode**: proceed with full workflow
- Always preserve existing ADRs and CHANGELOG entries

**STOP. Present Phase 1 deliverables to the user. Do not proceed to Phase 2 until the user confirms.**

---

## Phase 2: Structure

Goal: Design the documentation architecture and reading path.

Read `refs/project-types.md` → Recommended Documentation Structure table. Select the structure matching the identified project type.

Read `refs/templates.md` → README Required Elements and Scale Adaptation.

### Design Principles
| Principle | Practice |
|---|---|
| Progressive depth | README: 3-minute quickstart → deep docs: full design understanding |
| Single source of truth | One fact detailed in one place; others link to it |
| Bidirectional cross-references | Navigation bar at top of each doc links to related docs |
| Executable examples | Command examples can be copy-pasted and run |

### Output Language Rule
After language is confirmed in Phase 1, ALL generated content — headings, table labels, Mermaid node labels, prose — must use the chosen language consistently. If English: use imperative mood for instructions, present tense for descriptions.

**STOP. Present the proposed doc structure to the user. Do not proceed to Phase 3 until the user confirms.**

---

## Phase 3: Generation

Goal: Generate high-quality content where every claim has code backing.

### Workflow (per document)
1. Launch Explore agent to deeply analyze the corresponding code module
2. Extract key facts (params, configs, signatures, defaults)
3. Write content
4. Internally record the code source of every fact (not in the doc — for validation later)

Read `refs/project-types.md` → Type-Specific Fact Sources table.

Read `refs/templates.md` → Content Format Standards, Command Example Standards, Mermaid Best Practices.

### The Iron Rule

**Every parameter name, default value, command flag, route path, and version number MUST be extracted from actual code via grep. Never write from memory.**

**STOP. Present generated docs to the user for initial review. Do not proceed to Phase 4 until the user confirms.**

---

## Phase 4: Validation

Goal: Systematic correctness verification. This is the most critical phase — invest as much effort here as in generation.

### Launch Two Validation Agents in Parallel

Read `refs/error-patterns.md` for the full error pattern checklist and verification commands.

Read `refs/project-types.md` → Type-Specific Validation Checks table.

**Agent 1 — Code Consistency**:
- Filenames/script names vs actually existing files
- Env var names vs names used in code
- Command param names vs code-defined params
- Version numbers vs actual config files
- Type-specific checks from the reference table

**Agent 2 — Completeness & Cross-Reference**:
- Same fact described consistently across all docs
- All cross-reference links valid (`ls` verify target files exist)
- Recommended approaches consistent throughout
- Every public script/command/API has usage docs
- Dependency files include all actual imports

### User Confirmation Round
Summarize validation findings to user, focusing on:
- Factual errors found (list each one)
- Whether recommended approaches are correct
- Whether important information is missing

**STOP. Present validation results to the user. Do not proceed to Phase 5 until the user confirms.**

---

## Phase 5: Polish

1. **Fix all issues** found in validation
2. **Unify formatting**: table alignment, code block language tags, consistent heading levels
3. **Update dependency files**: add missing packages
4. **Commit**: clear commit message listing all changes

### Pre-Commit Checklist
- [ ] Every command example has correct syntax, param names verified from code
- [ ] Every table value verified from code
- [ ] Mermaid diagrams have no syntax errors
- [ ] Doc navigation links all valid (ls verify target files)
- [ ] Same value/config consistent across all docs
- [ ] FAQ code examples reference real project filenames and function names
- [ ] Dependency files complete
- [ ] .gitignore does not exclude docs directory
- [ ] README contains: project positioning, quick commands, tech stack, doc navigation + type-specific elements

---

## Anti-Patterns (Rationalization Table)

| Anti-Pattern | Agent May Think... | Why You Must Not Skip |
|---|---|---|
| Write params from memory | "I just read this file, I remember the value" | Memory degrades across turns. grep takes 1s and is authoritative |
| Fabricate FAQ | "These are common questions for this type of project" | Only record problems actually encountered — ask the user |
| Skip validation phase | "The content looks correct to me" | 40%+ of doc bugs are invisible to the author. Independent verification is non-negotiable |
| Cram everything into README | "Users want one file" | Layer docs — README is the entry point, not the encyclopedia |
| Assume recommended approach | "This is the default in code, so it must be recommended" | Defaults ≠ recommendations. Ask the user |
| Duplicate facts across docs | "Users might not read the other doc" | One detailed source + links elsewhere. Duplication rots |
| Ignore historical context | "Just document what exists now" | "Why this choice" is more valuable than "what was chosen" |
| Use generic FAQ templates | "This is a standard answer" | FAQ code examples must reference actual project file paths and function names |
| Skip cross-reference verification | "I'm sure the link is right" | Files get renamed. Always `ls` to verify |
| Ignore project type differences | "One template fits all" | Doc focus, validation methods, and command formats differ completely by type |
| Write docs and never update | "My job is done" | Docs are living documents. Recommend maintenance mechanisms |
| Skip user checkpoints | "I'll show them the final result" | Each phase builds on user confirmation. Skipping creates compounding errors |

---

## Documentation Maintenance

Recommend these mechanisms to the user:
1. **PR review check**: When params/API/config change, verify corresponding docs are updated
2. **CI checks**: Link checker for `docs/`, Mermaid syntax validator
3. **Centralized version management**: Frequently-changing values (versions, ports, image tags) defined in one place, referenced elsewhere
4. **Periodic review triggers**: Major releases, refactors, new team member onboarding
