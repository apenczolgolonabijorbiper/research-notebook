# Local execution, durable checkpoints and agent handoff

Status: proposed; depends on new Chelm capabilities, not delivered by this plan.
Primary route: Sites + active agent + existing local JSON/Git runtime + private checkpoints.

## 1. Scope and ownership

Use local Git for host transactions/history without a GitHub remote. Keep it outside application source. Reuse published Harness semantics and Chelm workers/cognition.

A temporary workspace is an execution location, not guaranteed durable storage. A complete runtime checkpoint is the persistence unit for those sessions. A persistent Cursor/Codex environment may keep its runtime directly and use the same checkpoint operation only for optional backup.

| Layer | Planned responsibility |
| --- | --- |
| Chelm #46 | Settled checkpoint creation, inventory, manifest, safe isolated restore, path/owner rebinding and logical checks |
| Chelm #47 | Product-neutral durable coordinator/object-store contract, session fencing, idempotent publication/recovery and reset generations |
| Chelm #48 | Validated deployment template, SudoLang/session procedures and actual acceptance |
| Notebook RN-08 | Authenticated bounded app/agent transfer binding using those public contracts |
| Notebook RN-09 | Concrete Sites D1 coordinator and R2 private objects, identity, requests/views and publishing |
| Harness | Existing provider, evidence, history and cognitive meanings; no new backup/host lifecycle owner |

No new GitHub API memory provider or mandatory owner server is needed. Persona1 supplies patterns for Sites interaction and explicit agent handoff; its private data/history and older transaction procedure are not copied.

## 2. What a checkpoint must preserve

The host must publish the authoritative inventory and format; the following requirements guide it:

- Canonical local Git object history/refs needed by retained snapshots and exact citations.
- Original observations/indexes and all supported logical memory, questions/hypotheses, journals/verification and guidance.
- Host intake registry, conversation, queued tasks, completed task results/receipts and retained task/evidence snapshots.
- Provider/configuration identities and recovery metadata required to reproduce the last settled state.
- App request acknowledgment watermark/IDs and linkage to the corresponding checkpoint revision.
- Required historical artifacts; avoid a shallow Git pack that breaks old source/task references.

This is not the same as report export, graph JSON export, selective processed-memory transfer, logical provider stream export or a source-code archive.

Exclude package payloads (restore their exact verified versions), credentials/tokens, session authority, live locks/PIDs/sockets and machine-specific executable paths. Git remotes, credential helpers, hooks and executable repository configuration must not be restored as trusted runtime configuration. Reconstruct safe config through host-approved rebinding.

First version exports only a settled runtime. Active assignments/leases and unresolved local publication/recovery refuse until their supported lifecycle is settled; a generic filesystem ZIP of active work is not a checkpoint. Keep queued user work. Any future in-flight checkpoint format requires its own version, recovery guarantees and tests.

Restore into a fresh owned directory, verify format/hash/size/path inventory, package compatibility and logical/Git completeness, then activate. Refuse symlink/traversal/hardlink escapes, missing content or unauthorized owner changes. Do not initialize empty memory when restore fails.

## 3. Minimal app/agent binding

The app provides a narrow authenticated binding for:
- Read current notebook generation, selected checkpoint/head and pending research requests.
- Obtain/renew/release a publishing session with a fencing token and finite expiry.
- Transfer checkpoint manifests/objects and captured input in bounded chunks.
- Publish a checkpoint/result projection conditionally against an exact base revision.
- Query publication status/receipt by stable request ID.
- Observe cancellation/reset and report reachable local cleanup.

These are proposed app-control/storage operations, not replacement cognitive worker APIs or arbitrary remote shell. Actual local analysis runs through existing Chelm contracts.

Use Site tools/MCP if current account/platform capability and tool connection are observed. Another supported bounded HTTP/file binding can be tested. Do not assume a process in the execution environment can call its owning chat's tool functions or that visiting a Site grants tool access. An explicit manual artifact import/export route can exist, but must not be claimed equivalent to an automated/live connected route.

No generic proxy may accept arbitrary commands, filesystem paths, module names, provider connection strings, principal fields or upload destinations. Keep trusted caller/owner/generation scope server-side. Browser code never receives storage/admin credentials.

## 4. Durable authority and publication

Default to one active publishing session per scratchbook. The durable coordinator owns:
- Notebook/runtime ID and reset generation.
- Selected checkpoint revision/content identity.
- Authorized owner/session, fencing token and finite expiry.
- Stable publication request ID and result acknowledgment.
- Pending app requests and their acknowledgment status.

The local host lock protects only that local copy. It cannot coordinate another restored copy. A local commit is not a hosted durable save.

Publication:
1. Persist user requests and captured inputs durably with stable IDs.
2. Agent obtains current authority/base revision, restores and verifies that checkpoint.
3. Agent executes bounded local work and settles host transactions.
4. Host creates a validated checkpoint and manifest.
5. Upload immutable checkpoint objects to private staging using bounded streaming/chunks.
6. Verify hashes/inventory and that all needed objects exist.
7. Atomically compare owner/session/fence/expiry/generation/base revision in the coordinator; select the new checkpoint and associated published result/projection metadata.
8. Acknowledge only request/input IDs included by that checkpoint; preserve later arrivals.
9. Return and retain the authoritative publication receipt; read-after-response-loss queries recover it.

D1/R2 do not share one transaction. Immutable upload followed by atomic coordinator selection is the proposed protocol; test compensation and orphan cleanup. Do not overwrite a mutable “latest.zip” as the correctness mechanism.

If projections are separate objects, upload them immutably with the checkpoint and choose their references in the same coordinator transaction. They cannot become visible as current before the corresponding checkpoint.

Proposed UI states: queued, executing, locally committed/checkpoint pending, durably saved, blocked, cancelled, stale and resetting. Never collapse local commit and durable publication into one success.

## 5. Failure and recovery

| Interruption | Required result |
| --- | --- |
| Before local completion | Last durable checkpoint remains authoritative; pending inputs remain replayable |
| After local commit, before checkpoint upload | Local-only progress; no durable completion acknowledgment |
| During upload | Partial/staged objects never become selected; retry bounded transfer |
| After upload, before head selection | Orphan candidate; retry conditional publication or prune later |
| After head selection, before response | Recover the same authoritative receipt; do not publish again under a new ID |
| Competing restored session | Conditional publication refuses; do not merge by last upload wins |
| Reset during execution/upload | Old generation cannot select head, publish results/learning or self-renew |
| Restore in new directory/OS principal | Host-approved rebinding, no inherited authorization |
| Missing object/hash/package mismatch | Explicit refused restore; never silently reseed or cite missing data |

Loss of an uncheckpointed local workspace means its local progress is not known durable. Restore the last confirmed checkpoint, replay still-pending app inputs idempotently and do not claim the lost local commit was recovered. Reconcile uncertain external actions before retrying; inference may have to run again.

A session must reacquire authority after expiry. Renewing authority cannot extend research budgets indefinitely. A later generation never implicitly adopts an earlier checkpoint.

## 6. Reset and retention

Reset first fences the generation and invalidates all old publication authority, then purges controlled checkpoint versions/staging, records, captures and reports. Reachable local copies use supported host sandbox hard reset.

Keep only content-free reset coordination data. Do not leave sensitive checkpoint versions in a hidden backup. Normal retention may prune redundant complete checkpoints only after proving current retained snapshots/citations/receipts remain resolvable; full reset purges all controlled versions.

An unreachable detached workspace or downloaded copy cannot be physically erased by a Site. Show durable purge and local cleanup separately, with pending/unknown cleanup where appropriate. Never report physical erasure or complete controlled cleanup while required acknowledgments are missing.

See [Sites and reset](SITES_AND_RESET.md).

## 7. Required release acceptance

- Actual Sites-authenticated tool/file transfer, not just a fixture upload.
- A genuine study under real Chelm/Harness, durable checkpoint, loss of first workspace and fresh-session restore/deepen.
- Exact old citations and host receipts remain valid; no duplicate source completion.
- Active/unresolved checkpoint refusal and safe owner/path rebinding.
- Corrupt/truncated/oversized objects, unsafe archive paths and secret/Git-config canaries.
- Two-session competition, lost publication response, local commit before upload and new input mid-cycle.
- Full reset, late old-session/checkpoint rejection and honest local-copy cleanup status.
- Clean source/release packaging without runtime data or credentials.
- Actual client paths/platforms and semantic reviews distinguished from deterministic checks.

Do not implement a private app backup shortcut that bypasses unfinished host contracts. Hosted durable-resume acceptance remains blocked on the published capability and observed app integration.
