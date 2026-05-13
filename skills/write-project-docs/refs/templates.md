# Documentation Templates and Standards

Load this file during Phase 2 (Structure) and Phase 3 (Generation).

## README Required Elements

### Universal (All Project Types)

1. **Project positioning**: One sentence — what it does and what it's built on
2. **Quick commands**: Install + core operation (train/start/build/deploy), one command each
3. **Tech stack overview**: Language, framework, core dependencies in a compact table
4. **Doc navigation table**: One row per sub-document, describing what it covers

### Type-Specific Additions

| Type | Additional README Requirements |
|---|---|
| ML | Performance metrics table, diff from baseline/original, deployment info, evolution timeline, literature references |
| Backend | API overview table (core endpoints), deployment architecture diagram, dependency service list, environment list |
| Frontend | Screenshot or demo link, browser/device compatibility, design system/UI framework info |
| SDK | Multi-language/multi-package-manager install, version compatibility matrix, changelog link, badges |
| CLI | Core command cheat sheet, config file example, shell completion install instructions |
| Go | godoc badge, Go version requirement, `go install` command, interface design overview |
| C/C++ | Build instructions (cmake/make), platform support matrix, ABI/API stability promise, memory management strategy |
| Rust | Cargo command cheat sheet, feature flag matrix, MSRV, unsafe usage notes |
| DevOps | Infrastructure topology diagram, environment list (dev/staging/prod), access & secrets management |
| Mobile | Supported platform/version matrix, app store links, build variant descriptions |
| Monorepo | Package index table, workspace tooling overview, contribution workflow per package |
| Data Eng | Pipeline catalog table, data freshness SLAs, connection/credential setup |

---

## Content Format Standards

| Content Type | Format |
|---|---|
| Parameter/config description | Table: Name / Type / Default / Description |
| Technology comparison | Table: Dimension / Option A / Option B / Recommendation |
| Architecture flow | Mermaid diagram (flowchart / sequence / class) |
| Command examples | Code block with full params, backslash line continuation |
| Design decisions | Prose paragraph with "Reason" and "Tradeoff" |
| API endpoints | Table or subsection: Method / Path / Params / Response |

---

## Command Example Standards

Rules:
- Include necessary env var prefixes
- One parameter per line (backslash continuation)
- Use `/path/to/` placeholders or real paths (be consistent throughout)
- Label as "recommended" or "historical reference"
- **Parameter names MUST be extracted from code** (never from memory)

```bash
# Python/ML
CUDA_VISIBLE_DEVICES="0" python train_script.py \
    --modelname ModelName \
    --dataPath /path/to/data \
    --lr 0.001

# Go
go build -ldflags "-X main.version=1.2.3" ./cmd/server
./server --config /etc/app/config.yaml --port 8080

# C/C++
cmake -B build -DCMAKE_BUILD_TYPE=Release -DENABLE_TESTS=ON
cmake --build build --parallel $(nproc)

# Rust
cargo build --release --features "async,tls"
cargo test --workspace --no-fail-fast

# Node/Frontend
npm run build -- --mode production
npx playwright test --project=chromium
```

---

## Mermaid Diagram Best Practices

- `flowchart TB/LR`: Data flow, processing pipelines
- `sequenceDiagram`: Component interaction timelines
- `classDiagram`: Class inheritance relationships
- `graph TB`: Module dependencies, version evolution
- `quadrantChart`: Multi-dimensional comparisons
- Use `style` to highlight recommended nodes

**Guidelines:**
- Keep each diagram under 15 nodes. If a flow needs more, split into overview + detail diagrams.
- Add a one-sentence text description above each diagram for accessibility.
- Test Mermaid syntax before committing (common pitfalls: unescaped special chars in node labels, missing semicolons in sequence diagrams).

---

## File Naming and Placement

- Place docs in `docs/` if the project already has one; otherwise create it
- `README.md` stays in project root
- Use lowercase-kebab-case: `quick-start.md`, `api-reference.md`, `data-pipeline.md`
- If a docs-as-code framework exists, follow its conventions for frontmatter and directory structure

---

## Audience Tone Guide

| Audience | Tone | Assumes Context | Example |
|---|---|---|---|
| Internal team | Casual, direct | Yes — shared jargon OK | "Run `make dev` and hit `/health`" |
| Open source users | Welcoming, thorough | No — explain everything | "This project provides... To get started, first install..." |
| API consumers | Terse, precise | Moderate — technical audience | "POST /api/v2/users — Creates a user. Returns 201 on success." |

Determine audience in Phase 1 user interview and apply consistently.

---

## Scale Adaptation

| Scale | Doc Volume | Focus |
|---|---|---|
| Small (<5k lines) | README + FAQ | Quick start, known limitations |
| Medium (5k-50k lines) | README + 2-3 topic docs | Core workflows, design decisions |
| Large (>50k lines) | Full 5-7 docs | All phases, architecture overview diagrams |
