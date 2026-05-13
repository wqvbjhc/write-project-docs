# Common Documentation Error Patterns

Load this file during Phase 4 (Validation). Use as a checklist for both validation agents.

## Error Pattern Table

| # | Error Pattern | Example | Prevention | Verification Command |
|---|---|---|---|---|
| 1 | **Wrong param/flag name** | `--model` written as `--modelname` | grep code for param definition | `grep -rn "add_argument\|flag\|option" src/` |
| 2 | **Wrong default/config value** | Port/timeout/weight from old version | grep code for exact assignment | `grep -rn "default=\|Default:\|:=" src/` |
| 3 | **Non-executable command example** | Incompatible param combination | Cross-verify param dependencies | Run the command or dry-run |
| 4 | **Wrong route/endpoint path** | `/api/user` written as `/api/users` | grep route registration code | `grep -rn "router\.\|@app\.\|@Get\|@Post" src/` |
| 5 | **A/B swapped** | "The one with X is version Y" (reversed) | Confirm from both filename and code | `grep -rn "version\|variant" src/` |
| 6 | **Missing branch in flow** | Data flow diagram omits an output | Check against return/export/registration | `grep -rn "return\|export\|register" src/` |
| 7 | **Missing dependency** | Required package not listed in dep file | grep all imports | `grep -rh "^import \|^from \|require(" src/ \| sort -u` |
| 8 | **Filename typo** | Project structure lists wrong filename | ls actual directory | `ls -la src/` |
| 9 | **Broken cross-reference** | Link target renamed but reference not updated | ls verify all `[xxx](file.md)` targets exist | `grep -roh "\[.*\](.*\.md)" docs/ \| sed 's/.*(\(.*\))/\1/' \| xargs -I{} ls {}` |
| 10 | **Generic FAQ** | Solution uses generic template not project code | FAQ must reference real file paths and function names | Review each FAQ answer for project-specific references |
| 11 | **Inconsistent values across docs** | Version/port/opset differs between docs | Single source of truth, link elsewhere | `grep -rn "v[0-9]\|port\|:[0-9]" docs/` |
| 12 | **Env var name mismatch** | Doc says `DB_URL`, code uses `DATABASE_URL` | grep code for env access | `grep -rn "os.getenv\|process.env\|os.Getenv\|env::" src/` |
| 13 | **Stale Docker image tag** | Doc says `v1.2`, actual is `v2.0` | Check Dockerfile/compose | `grep -rn "image:" docker-compose*.yml Dockerfile*` |
| 14 | **Stale API response format** | JSON response in doc differs from handler return | Compare with handler return statements | `grep -rn "json(\|JsonResponse\|JSON.stringify" src/` |

## Validation Agent Assignment

**Agent 1 — Code Consistency**: Check errors #1-6, #12-14 (facts that must match code).

**Agent 2 — Completeness & Cross-Reference**: Check errors #7-11 (structural integrity and consistency across docs).

## Universal Checks (Both Agents)

- Filenames/script names vs actual files that exist
- Env var names vs names used in code
- Command example param names vs code-defined param names
- Version numbers/dep versions vs actual config files
- Every `[link](target.md)` target file exists (verify with `ls`)
- Same fact described consistently across all docs
