# Research Notebook implementation plan

Status: proposed; awaiting explicit owner approval in [#1](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/1).
Prepared: 24 November 2026 (Europe/Warsaw).
Scope of this change: reviewable planning documents and GitHub issues only.

## 1. Product outcome and v1 boundary

Provide a small, generic reference application that shows real use of both Chelm-host and golem14-harness for research and metacognition. A user chooses a subject, receives a short structured study, inspects original supporting evidence and unresolved questions, then directs further investigation. A published Sites instance is a disposable scratchbook with an administrator full reset.

The example must show evidence evaluation and revision as well as summarization. It must remain understandable enough to copy into another application.

### Included

- One private scratchbook per installation/owner by default, with multiple studies and topics. A full scratchbook reset clears all of them.
- Topic/chapter outline, findings, citations, uncertainty, hypotheses, conflicts and open questions.
- Agent-provided web discovery and collection, user URLs and pasted source material.
- Normal observation analysis followed by separate memory-backed cognitive tasks.
- User-directed deepening, branching replies, new evidence and corrections.
- Shared local/hosted UI and portable report export.
- Cursor, Codex and explicitly loaded ChatGPT Work setup/reconnect workflows.
- Sites publishing with persistent app records and a connected external canonical runtime.
- Full reset of controlled data, including canonical history and old task artifacts in the app-owned sandbox.
- Upstream source tracking, dependency pinning and real end-to-end acceptance.

### Outside v1

Autonomous continuous crawling, unbounded background research, custom model serving, a new memory provider/database, multi-tenant shared editing, embedding infrastructure, a new identity provider, a generic command proxy and replacements for the upstream cognitive engine. Optional distributed MCP is not required for local operation.

## 2. Experience and defaults

A suggested first subject is “How can cities reduce summer heat?” It is broad enough for topic organization and disagreement without requiring a specialized domain model.

1. Enter subject, scope and language; optional as-of date and preferred sources.
2. Preflight shows the actual mode, executor, available browsing and persistent runtime.
3. Start a bounded collection. Proposed defaults: target 5 inspected sources, maximum 8 distinct source captures, 3–5 initial topics and up to 2 collection/cognition rounds.
4. Submit sources through Chelm. Show waiting-for-age, indexed and analyzed states honestly.
5. Process observations under Harness guidance through the negotiated worker contract.
6. Create a cognitive task over committed evidence and project its result into chapters.
7. Inspect evidence, conflicts, hypotheses, unanswered questions and concise next checks.
8. Choose Deepen, Challenge, Verify, Add source or Answer clarification; run another explicitly bounded cycle.
9. Compare the revised study with prior findings or export it.
10. In Admin, perform full scratchbook reset when the disposable data is no longer wanted.

Defaults are application policy, not upstream guarantees. Enforce a cumulative study collection/invocation ceiling as well as per-task bounds. Token/cost ceilings are enforceable only when measured executor APIs exist; unknown usage remains null. A user can explicitly extend a budget, but a new task must never silently renew one.

The app can queue work and show “waiting for an agent.” Neither a page click nor a read-model refresh proves that an agent has started inference.

## 3. Responsibility and authority

| Component | Owns | App must not duplicate |
| --- | --- | --- |
| Research Notebook | UX, study outline, source collection policy, budgets, projections, admin authorization | Canonical memory truth or cognitive validation |
| Chelm-host | Intake, runtime/provider selection, tasks, assignments, snapshots, receipts, leases/CAS and lifecycle | App chapters and source-discovery decisions |
| Harness | Cognitive procedure, logical memory, provenance/history rules, retrieval, schemas and validators | Host identity, deployment or UI |
| Cursor/Codex/Work executor | Actual browsing, extraction, reasoning and truthful provenance | Host authorization or trusted receipts |
| Sites | Hosted identity/access, Worker execution and D1/R2 resources | Python/Git execution |
| App gateway | Narrow transport from app calls to public Chelm operations | A new reasoning engine, task/lease implementation or delegation system |

Canonical cognitive memory remains authoritative under Chelm/Harness. D1 and app JSON contain study metadata and rebuildable projections plus app-owned collections; they are not an alternative canonical claim store. Every projection records its source task, result/work ID, binding, generation and generated time.

The default uses one app-owned guarded runtime for the scratchbook; studies map to scoped subjects/tasks. Do not claim hard security isolation between studies: the runtime/owner boundary is the authorization boundary. If multi-user use is later required, design separate scoped runtimes before enabling it.

## 4. Architecture and proposed implementation shape

Use TypeScript for application contracts/orchestration; a small React/Vite UI shared by local and hosted modes; a Node adapter/gateway for current executable upstream packages; and a Sites-supported Worker adapter. At RN-01, check the current supported Sites starter and settle build wiring before writing features. Avoid forcing upstream Node-only code into the Worker bundle.

Proposed code organization, to be created only after approval:

| Path | Planned responsibility |
| --- | --- |
| app/core/ | Study contracts, orchestration, budget and projection rules |
| app/ui/ | Notebook UI, evidence inspector and administration |
| app/adapters/ | HostClient interface, local CLI client and remote gateway client |
| gateway/ | Authenticated allowlisted application operations and runtime mapping |
| sites/ | Worker routes, identity, D1/R2 persistence and optional notebook MCP tools |
| profiles/research/ | App-owned purpose, guidance, vocabulary and source rules |
| prompts/ | Shared SudoLang workflows and surface-specific entry instructions |
| tools/ | Bootstrap, preflight, alignment check, export and release preparation |
| tests/fixtures/ | Small labelled deterministic source sets and acceptance scenarios |
| docs/ | Quickstarts, architecture, compatibility and retained acceptance |
| vendor/ | Verified package installation; exact tracking/bundling policy set in RN-03 |

Each adapter consumes public APIs rather than importing private validators, parsing canonical graph files or constructing leases. Instance IDs, snapshot IDs and policy/guidance IDs are opaque.

### Deployment modes

| Mode | App persistence | Canonical runtime | Executor |
| --- | --- | --- | --- |
| Fixture | Local JSON or Sites D1/R2 | None; synthetic data only | None |
| Local live | App-owned files through local API | Persistent guarded runtime with verified packages | Agent with local CLI/stdio tools |
| Connected Sites live | D1 records + R2 files | Persistent external runtime behind scoped HTTPS gateway | Agent using existing Chelm worker tools; optional Site tools |

Local and hosted modes share the same user concepts and API meanings. They need not have identical infrastructure. Work requires an explicitly loaded setup record and actual tool/storage access. Transient workspace disk by itself is not a persistence strategy.

A disconnected published notebook remains usable for inspecting retained app data and fixtures. Mutating live research shows a blocked/queued state until its configured host and executor are available. Do not silently answer from fixtures when live work fails.

## 5. State and research data

### Application records

- Scratchbook: owner/admin binding, generation, runtime binding, reset state and default budgets.
- Study: stable ID, subject/question, scope/as-of, language, topic outline, selected sources, budget and cycle history.
- Topic: stable app ID, title, parent/order and referenced result/finding IDs.
- Source capture: source/revision IDs, original and canonical URL, title, publisher/author when known, publication/access dates, captured text/quotes and their hashes, capture method and truncation.
- Cycle: collection receipts, analyzed-source status, task/assignment/result references, answered binding, pending count, stop reason, usage and model unknowns.
- Projection/export: included study/generation, evidence inclusion policy, host provenance and output schema.
- Admin reset: request ID, target generation, completion acknowledgments and a content-free durable receipt.

URL canonicalization never proves source identity/independence. Retain the URL actually fetched. An unknown publication date differs from the access date. Cite the captured excerpt as such, not a claim to retain an entire webpage.

Canonical host data includes claims/entities, original observations/evidence, processed ledger, questions, hypotheses, events/journals/verification and history. Keep their IDs, correction semantics and publication receipts under upstream ownership.

### Ingestion versus cognition

New source -> capture -> intake -> age eligibility -> observation analysis -> committed evidence -> cognitive task -> report.

A task cannot use pending raw intake as if it were analyzed memory. Source assertions, model-derived conclusions, unverified hypotheses and general model knowledge remain distinct. Repeated answers do not corroborate themselves.

Deepening first obtains missing evidence through normal intake/analysis, then explicitly continues/rebinds cognition. Preserve prior answers with their original evidence. Finite task continuation limits may require a linked new task; preserve conversation provenance and cumulative user budgets.

### Corrections

A changed source has a new revision and observation. Never overwrite old captured evidence or quotations. Re-evaluate dependent findings and retain the old result with an explicit change summary. A changed premise or stale binding requires renewed reasoning before learning publication.

## 6. Integration with latest upstream work

The reviewed default branches are newer than the published pair:

| Project | Latest source reviewed | Latest published version observed |
| --- | --- | --- |
| Harness | [00aba14b4964](https://github.com/apenczolgolonabijorbiper/golem14-harness/commit/00aba14b49641c3deb9d17d7cc327f6f8578f503) | 0.12.0 |
| Chelm-host | [8a0e533c6689](https://github.com/apenczolgolonabijorbiper/chelm-host/commit/8a0e533c6689f589dba1ec00dd82397d403b3a0b) | 1.2.0 |

The new public memory-provider boundary is merged in source; Chelm marks its exact development package unpublished and production-disabled. The app architecture must follow that ownership boundary now. Source-only contract testing may use an exact isolated development pair; production installation must use a supported published pair.

This plan's baseline expires as upstream moves. [The alignment policy](UPSTREAM_ALIGNMENT.md) is mandatory for every implementation slice. At implementation start, resolve current heads/releases again. On a newly published compatible provider pair, adopt it in the same release change across adapters, schemas, prompts, fixtures, CI and documentation. Do not build new application dependence on legacy physical graph layouts while waiting.

No upstream release or private runtime is upgraded by approval of this app plan. Optional SQLite is not a prerequisite.

## 7. Sites and admin reset

A Sites deployment executes a Cloudflare Worker. It cannot spawn Python, Git or the current host CLI. Plan an external persistent runtime and a narrow HTTPS gateway. D1/R2 hold app state and files, not a claim that the full upstream engine runs inside Sites.

See [Sites and reset](SITES_AND_RESET.md) for the authoritative reset inventory, server-side owner/admin requirements, generation fencing, resumable cross-store deletion and publication procedure.

Full reset clears all studies and their app-owned canonical memory/history. Ordinary host archive reset is insufficient because archived content is still retained. Reset must use supported sandbox hard reset, plus app-store and app-created archive cleanup inside an exact guarded ownership boundary. It preserves the deployment, administrators, dependency packages and connection configuration.

## 8. Implementation slices and order

| Slice | Issue | Depends on | Deliverable |
| --- | --- | --- | --- |
| RN-01 | [RN-01 / #2](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/2) | Approval | Define application boundaries, deployment modes and notebook contracts |
| RN-02 | [RN-02 / #3](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/3) | RN-01 | Keep dependency pins and integration continuously aligned with upstream sources |
| RN-03 | [RN-03 / #4](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/4) | RN-01, RN-02 | Build verified bootstrap, preflight and local runtime adapter |
| RN-04 | [RN-04 / #5](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/5) | RN-01, RN-02 | Define research profile, study structure and source provenance |
| RN-05 | [RN-05 / #6](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/6) | RN-03, RN-04 | Implement bounded web collection and observation intake |
| RN-06 | [RN-06 / #7](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/7) | RN-03, RN-04, RN-05 | Implement cognition, deepening, corrections and finite study cycles |
| RN-07 | [RN-07 / #8](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/8) | RN-01, RN-04, RN-06 | Build the notebook UX, evidence inspector and portable reports |
| RN-08 | [RN-08 / #9](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/9) | RN-02, RN-03, RN-06 | Add a scoped application gateway for hosted access to Chelm |
| RN-09 | [RN-09 / #10](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/10) | RN-07, RN-08 | Implement ChatGPT Work Sites persistence, identity and publication |
| RN-10 | [RN-10 / #11](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/11) | RN-03, RN-08, RN-09 | Implement administrator full reset with stale-worker fencing |
| RN-11 | [RN-11 / #12](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/12) | RN-03, RN-05, RN-06, RN-09, RN-10 | Write SudoLang agent workflows and three-client quickstarts |
| RN-12 | [RN-12 / #13](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/13) | RN-01, RN-02, RN-03, RN-04, RN-05, RN-06, RN-07, RN-08, RN-09, RN-10, RN-11 | Complete cross-client acceptance and release the verified sample |

Delivery checkpoints:

1. **Contracts and source alignment:** RN-01, RN-02. Evidence: ownership review, version negotiation and exact dependency/source snapshot.
2. **Real local research:** RN-03 through RN-06, plus the first part of RN-11. Evidence: captured sources -> host analysis -> grounded answer -> new evidence -> revised answer.
3. **Usable notebook and hosted connection:** RN-07 through RN-09. Evidence: the same workflow operates through a published Sites notebook and persistent gateway/runtime.
4. **Disposable scratchbook:** RN-10. Evidence: complete reset, concurrent/old-worker rejection and failure recovery across controlled stores.
5. **Reference-quality delivery:** RN-11, RN-12. Evidence: actual three-client onboarding, hosted deepening, reproducible release and semantic review.

Do not treat a fixture UI or successful deployment as a complete live application. Implement a narrow real end-to-end path before broadening UI polish. Documentation and reset fences should be designed early even though final acceptance depends on the hosted path.

## 9. Requirement-to-evidence map

| Requirement | Implementation | Mandatory evidence |
| --- | --- | --- |
| Both host and harness do real work | RN-03, RN-06, RN-08 | Authoritative intake/analysis/task/publication receipts |
| Grounded web research | RN-04, RN-05 | Inspected original content and exact retained citation resolution |
| Metacognition and revision | RN-06, RN-07 | Gap/hypothesis/conflict -> user-selected check -> revised evidence-backed finding |
| Topic organization | RN-04, RN-07 | Stable topics with result/finding references and exports |
| Latest upstream alignment | RN-02 and every issue | Exact source checks, impact review, current compatible release pins and drift gate |
| Cursor/Codex/Work usability | RN-11, RN-12 | Actual startup and resume record in each client |
| Sites publishing | RN-08, RN-09, RN-12 | Exact source/build/version provenance and live connected operation |
| Full reset | RN-10, RN-12 | Before/after inventory, old-worker rejection and recovery from interrupted deletion |
| No fabricated model evidence | RN-06, RN-12 | Original outputs, known/unknown config, separate semantic verdict |
| Portable report | RN-07, RN-12 | Export opens independently and accurately labels included evidence/restore scope |

## 10. Validation and completion

Use deterministic fixtures for mechanics and fault cases; keep them distinct from actual LLM/browser execution. Retain original model output before validation/review, exact request/context and source bindings, dependency identities, host receipts, hashes, model/settings unknowns and semantic verdicts with reasons. Passing structural validation is not proof of relevance, entailment or truth.

Required scenarios include: source deduplication/revision, inaccessible sources, truncation, source age, conflicting claims, superseded premises, branching replies, insufficient evidence, finite budget, cancellation, stale snapshots/guidance, unknown commit outcome, restart/reconnect, unauthorized operations, malicious source instructions, import/export and reset with old workers/partial failures.

Observe local bootstrap on Windows/Linux/macOS, actual Cursor/Codex/Work research, and a connected published Sites round plus deepening and reset. Test only isolated scratch runtimes and synthetic/disposable data for destructive cases. User-specific IIS deployments are not required for v1; never imply generic CI validates one.

Before release:
- All slice acceptance criteria have evidence or remain visibly blocked.
- Latest source changes are assessed and the newest supported compatible published pair is integrated.
- No vendor semantics were forked and no private runtime data is packaged.
- Hosted/local behavior and provenance survive reconnect.
- Full reset purges controlled data and rejects stale writes.
- Docs and SudoLang procedures match actual shipped commands.
- Universal support claims are withheld while any required client route is unrun.

## 11. Approval record

**Pending.** The owner reviews this plan and issues in the repository. Explicit approval may select/amend decisions before implementation begins. Merging documentation is not implicit authorization to start coding or deploy. This plan introduces no runtime, background task, scheduled CI, credential, Site or deletion operation.
