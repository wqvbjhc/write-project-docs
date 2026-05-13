# Project Types Reference

Load this file after identifying the project type in Phase 1.

## Type Identification

| Project Type | Typical Directory/File Signatures |
|---|---|
| ML/Data Science | `train*.py`, `model/`, `dataset/`, `loss/`, `configs/`, `.pt/.ckpt/.h5` files |
| Backend Service | `routes/`, `controllers/`, `middleware/`, `schema/`, `app.py/main.go/server.ts` |
| Frontend App | `components/`, `pages/`, `hooks/`, `store/`, `package.json` + frontend framework deps |
| SDK/Library | `src/` + `examples/`, `__init__.py`/`index.ts` exports, `setup.py`/`package.json` as publish config |
| CLI Tool | `commands/`, `bin/`, cobra/click/yargs frameworks, `man/` pages |
| Go Project | `go.mod`, `cmd/`, `internal/`, `pkg/`, `*.go` files |
| Infrastructure/DevOps | `terraform/`, `k8s/`, `helm/`, `Dockerfile`, `.github/workflows/`, `ansible/` |
| C/C++ System | `CMakeLists.txt`/`Makefile`/`meson.build`, `include/` + `src/`, `.h/.hpp/.c/.cpp` files |
| Rust Project | `Cargo.toml`, `src/main.rs`/`src/lib.rs`, `benches/`, `build.rs` |
| Mobile | `ios/`, `android/`, `lib/` (Flutter), `*.xcodeproj`, `build.gradle` |
| Fullstack | Frontend + backend signatures coexist, typically `frontend/` + `backend/` or `client/` + `server/` |
| Monorepo | `nx.json`/`turbo.json`/`pnpm-workspace.yaml`/`lerna.json`/`WORKSPACE`(Bazel), multiple `packages/` or `apps/` |
| Data Engineering/ETL | `dags/`(Airflow), `models/`(dbt), `pipelines/`, `etl/`, `spark/`, `*.sql` + orchestration config |

> A project may be a hybrid (e.g., "ML training + CLI inference tool"). Use the primary type, supplement with secondary type requirements.

---

## Exploration Directions (Phase 1.4 — Launch 2-3 Explore Agents in Parallel)

| Type | Agent A Focus | Agent B Focus | Agent C Focus |
|---|---|---|---|
| ML | Training/inference entry points and args | Data pipeline and storage | Model architecture and deployment |
| Backend | API routes and middleware | Data models and storage | Auth and deployment |
| Frontend | Page routing and component tree | State management and data fetching | Build and deploy config |
| SDK | Public API and types | Internal implementation and deps | Examples and tests |
| CLI | Command registration and args | Core business logic | Config and plugin system |
| Go | Public API (pkg/) and interfaces | Internal impl (internal/) | Build/test/deploy |
| C/C++ | Public headers and API | Build system and dependencies | Platform adaptation and tests |
| Rust | Public API (lib.rs) and traits | Internal modules and deps | Build/feature flags/tests |
| DevOps | Resource definitions and topology | CI/CD pipelines | Monitoring and alerting |
| Monorepo | Package structure and dependency graph | Shared tooling and configs | Build orchestration and CI |
| Data Eng | DAG/pipeline definitions and scheduling | Data sources and sinks | Data quality and monitoring |

---

## Recommended Documentation Structure (Phase 2)

| Type | Recommended Docs |
|---|---|
| ML/Data Science | README + Quick Start + Data Pipeline + Architecture + Training & Deployment + FAQ (+ SAD overview for large projects) |
| Backend Service | README + Quick Start + API Reference + Architecture + Deployment & Ops + FAQ |
| Frontend App | README + Quick Start + Component Docs + State & Data Flow + Build & Deploy + FAQ |
| SDK/Library | README + Quick Start + API Reference + Internal Architecture + Migration Guide + Contributing + FAQ |
| CLI Tool | README + Install & Usage + Command Reference + Configuration + Plugin Development + FAQ |
| Go Project | README + Quick Start + API/pkg Reference + Internal Architecture + Deploy & Ops + Contributing + FAQ |
| C/C++ System | README + Quick Start + API Reference + Build System Guide + Architecture + Platform Porting + FAQ |
| Rust Project | README + Quick Start + API Reference (rustdoc) + Architecture + Feature Flags + Contributing + FAQ |
| DevOps/Infra | README + Quick Start + Architecture + Runbook + Alerting & Incident Response + FAQ |
| Mobile | README + Quick Start + Architecture + Platform Adaptation + Build & Release + FAQ |
| Fullstack | README + Quick Start + Frontend Docs + Backend Docs + Deployment + FAQ |
| Monorepo | README + Quick Start + Package Index + Shared Tooling + Dependency Graph + Contributing + FAQ |
| Data Eng/ETL | README + Quick Start + Pipeline Catalog + Data Lineage + Schema Reference + Ops & Monitoring + FAQ |

> These are starting points. Scale down for small projects (README + FAQ), scale up for large ones (7+ docs).

---

## Type-Specific Fact Sources (Phase 3 — Where to Extract Facts)

| Type | Key Fact Sources | Must grep-verify |
|---|---|---|
| ML | `argparse` defs, loss/optimizer code, `forward()` | Param names, defaults, loss weights, optimizer config |
| Backend | Route registration, middleware chain, ORM models | Route paths, HTTP methods, status codes, field types |
| Frontend | Component props/types, router config, store defs | Prop names/types, route paths, action names |
| SDK | Public API signatures, type defs, export lists | Method names, param types, return types, exceptions |
| CLI | Command registration, flag defs, config schema | Command names, flag names/shorthands, defaults, env vars |
| Go | Exported funcs/interfaces, go.mod, `cmd/` entry | Func signatures, interface methods, version constraints, build tags |
| C/C++ | Header declarations, CMakeLists.txt, `#define` macros | Func signatures, compile options, macro values, link libraries |
| Rust | `pub` funcs/structs/traits, Cargo.toml, feature gates | Public API signatures, feature flag names, dep versions, MSRV |
| DevOps | Terraform variables, k8s manifests, CI config | Resource names, ports, image names, secret names |
| Mobile | AndroidManifest, Info.plist, build.gradle/Podfile | Permissions, min versions, dep versions |
| Monorepo | Workspace config, package.json per package, shared deps | Package names, version policies, dep constraints |
| Data Eng | DAG definitions, schema files, connection configs | Table/schema names, schedule expressions, connection IDs |

---

## Type-Specific Validation Checks (Phase 4)

| Type | Must Verify |
|---|---|
| ML | Param defaults vs argparse, loss weights vs code, optimizer config vs code, data shape vs forward() |
| Backend | Route paths vs registration, HTTP methods vs handler, status codes vs returns, middleware order vs config |
| Frontend | Component props vs TS types, route paths vs router config, store action names vs code |
| SDK | API method names vs exports, param types vs signatures, return types vs impl, exception types vs throw |
| CLI | Command names vs registration, flag names vs defs, flag shorthands vs code, defaults vs code |
| Go | Exported func names vs code, interface methods vs defs, go.mod versions vs actual, build tags vs code |
| C/C++ | Func signatures vs header declarations, CMake option names vs CMakeLists.txt, macro values vs `#define`, link lib names vs target_link_libraries |
| Rust | pub API vs lib.rs exports, feature names vs Cargo.toml [features], dep versions vs Cargo.toml, unsafe blocks vs documented safety |
| DevOps | Resource names vs manifests, ports vs config, image:tag vs Dockerfile/compose, secret names vs references |
| Monorepo | Package names vs workspace config, shared dep versions vs lockfile, cross-package imports vs exports |
| Data Eng | Table names vs schema defs, DAG schedule vs config, connection IDs vs env/secrets |

---

## Type-Specific Risk and Emphasis (Adaptation Guide)

| Type | Biggest Doc Risk | Most Important Validation | Diagram Emphasis |
|---|---|---|---|
| ML | Training params inconsistent with code | argparse defaults, loss config | Data flow, model architecture |
| Backend | API docs diverge from actual behavior | Route paths, request/response format | Layered architecture, sequence diagrams |
| Frontend | Component usage contradicts props | TypeScript types, router config | Component tree, state flow |
| SDK | API signatures don't match implementation | Public method signatures, type defs | Class diagrams, call graphs |
| CLI | Command/flag docs don't match code | Command registration, flag defs | Command hierarchy, config priority |
| Go | Exported API diverges from godoc | Interface signatures, go.mod versions | Package dependency, request flow |
| C/C++ | Header declarations inconsistent with impl/build | Func signatures vs headers, CMake options | Module dependency, memory lifecycle |
| Rust | API/feature docs inconsistent with Cargo.toml | pub API vs exports, feature flags | Trait relationships, module hierarchy |
| DevOps | Deployment docs diverge from actual config | Manifest resource names/ports/images | Infra topology, CI/CD pipeline |
| Monorepo | Package docs diverge from workspace config | Package names, cross-package deps | Dependency graph, build pipeline |
| Data Eng | Pipeline docs diverge from actual schedules/schemas | DAG schedules, table schemas | Data lineage, pipeline flow |
