# Research Notebook implementation plan

Status: revised proposal; explicit approval to implement remains pending in [#1](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/1).
Revised: 24 November 2026 (Europe/Warsaw).
This change contains planning documents and issue updates only.

## 1. Product outcome

Provide a small generic sample of real agentic research and metacognition using both Chelm-host and golem14-harness. A user supplies a subject, obtains a short source-grounded study, inspects conflicts/hypotheses/gaps and directs deeper investigation. A published Sites notebook is a disposable scratchbook with owner/admin full reset.

### v1 scope

- One private scratchbook/owner per installation by default, multiple studies/topics.
- Actual inspected web sources, user URLs and pasted evidence; bounded collection.
- Observation analysis, then separate grounded cognitive tasks.
- Topics/chapters, evidence inspector, linked follow-ups, hypotheses and revisions.
- Shared responsive local/Sites UX and HTML/Markdown/app-JSON report export.
- Actual Cursor/Codex/Work setup, agent handoff and fresh-session reconnect.
- Local Git execution with optional durable backup on persistent machines.
- Private complete-runtime checkpoint/resume for temporary agent environments.
- Sites app persistence and admin reset across controlled runtime/checkpoint data.
- Latest-source alignment, exact supported dependency pins and actual acceptance.

Outside v1: autonomous continuous crawling, unlimited research, custom model hosting, mandatory remote MCP, a new memory database/provider, multi-user collaboration, mandatory GitHub memory storage and an always-on gateway. GitHub remains suitable for distributing application source; that does not make it the memory store.

## 2. Primary architecture

**Sites + active agent + local Git + private durable checkpoints.**

A Site cannot spawn the current Python/Git toolchain, but that does not imply a dedicated server. The active Work/Codex/Cursor execution environment runs Chelm + Harness locally. Its private runtime has its own instance Git repository outside the app checkout and outside vendor packages.

A persistent local machine can retain that runtime directly. A temporary Work workspace needs a verified durable checkpoint and explicit restore; scratch disk is not assumed to survive a session. An always-on remote service/gateway is an optional route, not the default.

| Layer | Authority |
| --- | --- |
| Research Notebook | Subject/outline/budgets, durable user input, UX/projections, reset authorization and concrete storage binding |
| Chelm-host | Intake, local runtime/tasks/leases/receipts/provider selection, complete-runtime checkpoint/restore and session coordination contract |
| Harness | Logical memory, evidence/history semantics, retrieval, cognitive procedure and validation |
| Agent environment | Actual browsing, reasoning, local Node/Python/Git execution and truthful model provenance |
| Sites Worker + D1 | Hosted auth, app records/pending requests, session coordinator, selected checkpoint pointer and published views |
| Private R2/object storage | Immutable runtime checkpoints, captured files and report artifacts; not a second canonical claim database |

Canonical knowledge remains the host/Harness runtime. D1 never independently changes claims. A selected checkpoint is a durable copy of that complete runtime, with exact local Git history and required host operational state. App projections refer to the selected checkpoint and original host task/result/snapshot.

The generalized deployment template belongs to Chelm, with product-neutral contracts. The app implements Sites identity/D1/R2 bindings. No new Harness requirement is identified: its existing provider boundary already delegates backup/lifecycle to hosts.

## 3. Experience and bounded defaults

Example subject: “How can cities reduce summer heat?”

1. Enter subject, scope/language and optional as-of date.
2. Preflight reports actual execution mode, agent access, browsing/tools, runtime package versions and durable storage.
3. Start collection: target 5 inspected sources, maximum 8 captures, 3–5 initial topics and up to 2 research rounds by default.
4. Persist requests/source captures with stable IDs before temporary execution; show queued until an agent picks up work.
5. Agent obtains the current session authority and restores the last confirmed checkpoint, or explicitly initializes a new empty notebook.
6. Submit sources through Chelm, respect minimum source age, process observations through negotiated worker APIs and validate/publish locally.
7. Run memory-backed cognition, produce topic projections and proposed follow-ups.
8. At a settled boundary create and durably publish a verified full-runtime checkpoint. Only then show the result as durably saved and acknowledge processed app requests.
9. User selects Deepen/Challenge/Verify/Add evidence/Answer clarification. A later session restores and continues under finite budgets.
10. Export or use Admin full reset.

These are proposed product defaults, not upstream guarantees. Per-task bounds and cumulative study bounds both apply. Creating a new linked task cannot silently replenish a user budget. Unknown inference token/cost values stay null.

While no agent is connected, the Site displays the last durably saved study and accepts durable pending requests. It does not claim that a click starts Work or guarantees background cognition. Optional scheduling requires a separately supported and explicitly configured executor.

## 4. Runtime and data separation

| Kind | Location/ownership |
| --- | --- |
| App source and reusable instructions | This repository and exact Sites deployment source |
| Installed package code | Verified, immutable vendor packages |
| Canonical memory | Private local runtime/instance Git repository, no remote required |
| Host state | Private local runtime outside instance Git, included according to checkpoint inventory |
| Durable copy for temporary sessions | Private versioned full-runtime checkpoint objects |
| App metadata, pending input, projections and coordinator | Local app store or Sites D1 |
| Captures/imports/exports | Private app file store/R2 |
| Credentials and environment identity | Host/Sites runtime configuration, excluded from checkpoint/source/report |

One publishing agent per scratchbook is the default. Study IDs narrow research scope but do not enforce separate user security domains; the scratchbook/runtime owner is the boundary.

### Proposed code layout

| Path | Purpose |
| --- | --- |
| app/core/ | Study contracts, budgets, projection/orchestration |
| app/ui/ | Notebook, evidence inspector, administration |
| app/adapters/ | Public local Chelm client and app/agent transfer binding |
| sites/ | Worker auth/routes/tools, D1 coordinator and private R2 storage adapter |
| profiles/research/ | App-owned purpose/profile/vocabulary |
| prompts/ | SudoLang setup, process, checkpoint, reconnect and research flows |
| tools/ | Bootstrap/preflight/source drift/report/release helpers |
| tests/fixtures/ | Labelled small deterministic corpora and fault scenarios |
| docs/ | Architecture, quickstarts, compatibility and retained evidence |
| vendor/ | Verified upstream install according to release policy |

Use a small shared TypeScript core and React/Vite UI; confirm current supported Sites starter/build wiring in RN-01. Do not bundle Node/Python/Git-specific runtime code into the Worker or copy private validators.

## 5. Source and study contracts

A scratchbook records owner/admin binding, reset generation, selected checkpoint revision and session status. A study records question/scope/as-of/language, topic outline, source IDs, cumulative budgets and cycles.

A source capture records stable source/revision IDs, original and canonical URLs, publisher/title/author when known, publication/access dates, exact captured text/quotes and hash, capture method, truncation and failure. URL normalization does not prove identity/independence. Unknown publication time is not the access time.

A cycle records app request IDs, source intake/analysis receipts, task/assignment/result IDs, answered snapshot/guidance binding, pending count, stop reason and known/unknown model usage. In hosted mode, it additionally records durable checkpoint revision and publication acknowledgment.

Reports/outlines are projections, never a second canonical store. Every projection cites the original result/finding/snapshot and reset generation. App JSON export is distinct from a full runtime checkpoint and logical-memory provider transfer.

### Ingestion and cognition

Capture -> durable pending input -> host intake -> age eligibility -> observation analysis -> committed evidence -> cognitive task -> local result -> durable checkpoint/publication -> visible saved study.

Pending intake is not analyzed memory. Source assertions, derived findings, hypotheses and general model knowledge stay distinguishable. Duplicate/syndicated sources and repeated model answers do not count as independent corroboration.

### Deepening and corrections

Obtain missing evidence, submit/process it normally, then explicitly continue/rebind cognition. Preserve older answers and evidence. Changed source content receives a new revision/observation; never overwrite original quotations. Re-evaluate dependent findings, label uncertainty and show concise changes.

Learning is a separately authorized proposal. Stale premises require renewed reasoning, not a mechanical merge. Finite task continuation/deadline limits may require a linked fresh task while retaining cumulative study budget and provenance.

## 6. Checkpoint and handoff design

[Runtime checkpoints](RUNTIME_CHECKPOINTS.md) defines the detailed contract and fault acceptance.

The primary route restores the existing local JSON/Git backend. It does not require a GitHub API provider, a GitHub memory repository, always-on service, new knowledge database or new delegated cognition protocol.

The app/agent binding supplies narrow authenticated operations for pending request/input transfer, session coordination, checkpoint object transfer and result publication. Site MCP is one supported route when actually available; another tested narrow binding may be used. A shell process cannot automatically invoke tools available only to its owning chat. Tool/identity/file-transfer access must be observed during setup.

Host checkpoint creation is initially at settled boundaries: no active assignments/processing lease or unresolved local publication recovery. Required host data includes retained task/citation snapshots, authoritative receipts, queues/intake and conversation. Copying only memory/graph.json or even the entire instance Git repository is insufficient.

Immutable checkpoint objects are verified before an atomic conditional head/projection selection. Owner/session token, expiry, reset generation and base revision are checked at that durable publication point. A local Git commit is not yet a durable hosted save. Pending requests are acknowledged only after successful publication, preserving any new input that arrived mid-cycle.

The app must retain original hosted inputs until acknowledgment. If a workspace is lost after a local commit but before checkpoint publication, resume from the last durable checkpoint and replay unacknowledged inputs idempotently. Do not fabricate recovery of a lost local commit. Unknown external action outcomes require authoritative reconciliation before retry.

## 7. Deployment modes

| Mode | Execution/persistence | Readiness |
| --- | --- | --- |
| Fixture | Supplied synthetic study; local or Sites app storage | No real cognition; prominently labelled |
| Persistent local | Real local CLI/stdio + own Git runtime | No GitHub remote/server; optional checkpoint backup |
| Sites + temporary agent | Restore local runtime, execute, publish full checkpoint to private storage | Requires shipped host checkpoint/fencing/template and working agent transfer binding |
| Optional remote service | Existing owner-hosted Chelm route | Later/optional, never a prerequisite |
| Optional GitHub memory | Consumer-selected backup/integration | Deferred; no adapter required by this plan |

A Site remains readable when the agent is absent and displays pending work honestly. No live failure silently substitutes a fixture. Public app source may be obtained from GitHub without storing private runtime there.

## 8. Current upstream baseline and new dependencies

Reviewed 24 November 2026:

| Project | Source reviewed | Published package observed |
| --- | --- | --- |
| Harness | [f3248e17de50](https://github.com/apenczolgolonabijorbiper/golem14-harness/commit/f3248e17de50646484a525c951cf7e48c59efcd0) | 0.13.0 |
| Chelm | [cd14af43ddf7](https://github.com/apenczolgolonabijorbiper/chelm-host/commit/cd14af43ddf79ef57719c65d8864e42513ed26df) | 1.3.0 |

Provider support is now published and production-enabled for explicit JSON/Git selection. The prior 0.12.0 development-package gate is obsolete. Checkpoint/session support is new and must not be inferred from this release.

Upstream host work:
- [Chelm #45](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/45): portable local-session epic.
- [Chelm #46](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/46): complete-runtime checkpoint and safe restore.
- [Chelm #47](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/47): durable publication, session fencing and reset generations.
- [Chelm #48](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/48): vendor-tested agent/Sites-style deployment template.

No Harness issue is created: current logical/provider/cognitive semantics already fit. If implementation identifies an actual neutral contract gap, raise a narrow Harness issue then rather than moving host lifecycle responsibilities into Harness.

[Upstream alignment](UPSTREAM_ALIGNMENT.md) applies to every slice. Resolve latest commits/releases before implementation and before merge/release. Require the latest compatible published checkpoint-capable host/harness pair for hosted resume acceptance; no custom app backup script or private vendor patch can bypass that gate. Optional SQLite remains unnecessary.

## 9. Implementation slices and dependencies

| Slice | Issue | Notebook prerequisites | Host prerequisites |
| --- | --- | --- | --- |
| RN-01: Define application boundaries, deployment modes and notebook contracts | [#2](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/2) | Implementation approval | Existing supported pair |
| RN-02: Keep dependency pins and integration continuously aligned with upstream sources | [#3](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/3) | #2 | Existing supported pair |
| RN-03: Build verified bootstrap, preflight and local runtime adapter | [#4](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/4) | #2, #3 | Existing supported pair |
| RN-04: Define research profile, study structure and source provenance | [#5](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/5) | #2, #3 | Existing supported pair |
| RN-05: Implement bounded web collection and observation intake | [#6](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/6) | #4, #5 | Existing supported pair |
| RN-06: Implement cognition, deepening, corrections and finite study cycles | [#7](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/7) | #4, #5, #6 | Existing supported pair |
| RN-07: Build the notebook UX, evidence inspector and portable reports | [#8](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/8) | #2, #5, #7 | Existing supported pair |
| RN-08: Integrate agent execution with durable runtime checkpoints and session fencing | [#9](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/9) | #3, #4, #7 | [Chelm #46](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/46), [Chelm #47](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/47), [Chelm #48](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/48) |
| RN-09: Implement ChatGPT Work Sites persistence, identity and publication | [#10](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/10) | #8, #9 | Existing supported pair |
| RN-10: Implement administrator full reset with checkpoint and stale-session fencing | [#11](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/11) | #4, #9, #10 | [Chelm #46](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/46), [Chelm #47](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/47) |
| RN-11: Write SudoLang agent workflows and three-client quickstarts | [#12](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/12) | #4, #6, #7, #9, #10, #11 | [Chelm #48](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/48) |
| RN-12: Complete cross-client acceptance and release the verified sample | [#13](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/13) | #2, #3, #4, #5, #6, #7, #8, #9, #10, #11, #12 | [Chelm #45](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/45), [Chelm #46](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/46), [Chelm #47](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/47), [Chelm #48](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/48) |

Delivery checkpoints:
1. Contracts/source alignment, RN-01/02; design local/durable status and coordinator boundaries.
2. Real local research, RN-03..06; actual source intake/analysis/grounded answer and revision.
3. Shared notebook, RN-07; visible evidence and stable projections.
4. Host checkpoint/fencing/template + RN-08/09; actually published Sites study with durable fresh-session resume.
5. RN-10 reset; purge checkpoint history and reject all old sessions/copies.
6. RN-11/12; three-client quickstarts, clean package/release and requirement-level evidence.

Do not close the epic for having written this plan or rendered fixtures. Avoid claiming a universally supported client/hosted route that has not been observed.

## 10. Acceptance and evidence

| Requirement | Mandatory evidence |
| --- | --- |
| Genuine host+harness | Normal intake/analysis/task validation/publication receipts |
| Grounded research | Inspected exact captures and retained source-bound citations |
| Metacognition | Gap/hypothesis/conflict -> user-selected check -> revised finding |
| Durable temporary execution | Original workspace unavailable; fresh session restores full runtime and deepens study |
| Publication integrity | Competing sessions, lost upload/head responses, commit-before-upload crash and new input mid-cycle |
| Reset | Durable purge inventory, late old-session rejection and honest local-copy cleanup status |
| Usability | Actual Cursor/Codex/Work setup, tool binding and reconnect |
| Sites publication | Exact source/build/version provenance, authenticated live transfer and saved-state display |
| Latest upstream | Exact current source/package/capability review and adoption gates |
| Report portability | Independent exported HTML/Markdown and accurately scoped JSON |

Use deterministic fixtures for mechanics/faults, actual agent output for cognition and separate semantic verdicts for meaning. Retain original responses before review, exact request/context/source bindings, hashes, checkpoint and host receipts, model/settings unknowns and limitations. A structurally valid citation does not prove entailment or relevance.

Required faults include source revisions/deduplication, source age/truncation/access failure, branching questions, stale premises, budget/cancel/expiry, auth/path attacks, active lease checkpoint refusal, missing checkpoint objects, partial upload, lost response, competing sessions, reset/reconnect and malicious imported content.

Test local package setup on Windows/Linux/macOS and actual client routes. A real Sites round/deepen/fresh-session resume/reset is mandatory. Clean packages omit studies, checkpoints, credentials, node_modules and personal Site IDs. Normal upgrade never resets data.

## 11. Approval

Architecture preferences in this revision reflect the owner's direction. **Implementation approval remains pending.** Approve/amend this plan explicitly in #1 or PR #14 before coding, deployment, schedules or destructive operations. Merging planning documentation alone is not approval to implement.
