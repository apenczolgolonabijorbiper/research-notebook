# Sites deployment and scratchbook full reset

Status: revised proposal. No Site, runtime, credential, schedule or deletion is created by this plan.

## 1. Hosting model

The default is **Sites + active agent + local Git + private durable checkpoints**. A dedicated service and a GitHub memory repository are optional, not prerequisites.

Sites runs Worker JavaScript; the current host executable path uses Node/Python/Git. Run those in the active Work/Codex/Cursor environment, keeping the canonical memory in a private local instance Git repository separate from app source. Sites stores durable app records/checkpoints and displays the latest saved study while no agent is connected.

| Component | Location |
| --- | --- |
| Notebook UI/authenticated routes | Sites Worker and browser |
| Studies, pending requests, coordinator/fences and selected checkpoint head | D1 |
| Full runtime checkpoint objects, sources/imports/reports | Private R2 |
| Real Chelm/Harness execution and local Git | Active agent execution environment |
| Canonical saved state | Validated selected checkpoint of the host runtime |
| Source distribution | GitHub app repository and exact Sites source; never private runtime data |

D1 is not a replacement canonical memory provider. R2 stores checkpoint objects; it is not a claim that the host runs in Workers. A page click can persist a request but cannot call the private Work scheduler or guarantee agent inference.

Fixture mode remains instant to explore and prominently labelled. Persistent local-agent mode needs no cloud checkpoint store; the hosted temporary-session route needs the host features tracked in Chelm #45–#48 and a verified tool/transfer binding.

## 2. Agent connection and identity

Use narrow supported app tools/HTTP/file transfer for pending inputs, checkpoint transfer, session authority and publication. Test actual access: a tool available to the chat is not automatically callable from a shell process. Prefer the platform-supported Site MCP/plugin route when available; no separate distributed Chelm MCP service is mandatory. A manual artifact path must be labelled separately.

Sites request identity scopes user-owned notebook operations. Start owner-private and preserve audience on deployment. Site-scoped user IDs are authoritative only through the platform's trusted request path. Missing identity or possession of an app ID does not confer authorization.

Configure administrators through deployment/operator configuration, never “first visitor becomes admin.” Every route/tool checks owner/admin scope server-side. Use authenticated same-origin POST, origin/CSRF checks as appropriate, input limits and duplicate-submission protection for mutations.

Keep credentials in runtime settings, outside code/checkpoints/client bundles/reports. Do not issue arbitrary command/path/module/provider proxies. Optional remote service/GitHub adapters require explicit configuration and may not become hidden primary dependencies.

## 3. Persistence and publication

Use D1 for durable app requests, metadata, reset/session authority and selected checkpoint/projection references. Use R2 for private immutable bounded objects. Browser storage holds preferences only.

A full runtime checkpoint preserves canonical history and host state, not just graph JSON or the instance repository. The host creates/restores it at a safe settled boundary. [Runtime checkpoints](RUNTIME_CHECKPOINTS.md) covers inventory, transport, coordination, failures and acceptance.

A local Git commit is only local completion. Upload/verify checkpoint and projection objects, then conditionally select their references in one coordinator transaction using generation, fence, owner/session, expiry and base revision. Keep new input and unacknowledged request IDs intact. Only then display “durably saved.”

Data survives normal redeploy/browser changes. A missing checkpoint is not permission to create a new instance silently. Fixture loading is explicit; schema migrations and deployment never seed old research back into a cleared notebook.

### Publishing the application

1. Select the reviewed GitHub app commit and prepare a sanitized code-only source snapshot.
2. Reuse or create Site identity only during authorized implementation/publication.
3. Record exact returned project ID in the deployment checkout's .openai/hosting.json. Reusable sample source has no personal/fabricated ID.
4. Declare D1/R2, configure owner/admin and runtime values through supported tools.
5. Generate/review bounded schema-only migrations; keep fixture/seed loading separate.
6. Build the supported Worker ES module at dist/server/index.js with default fetch handler and shared UI assets. No Python/Git process spawning in the Worker bundle.
7. Validate source/build, synchronize/package, save/deploy through supported Sites tooling with audience preserved.
8. Record GitHub source commit, Sites source commit, archive, saved version and deployment status.
9. Validate actual auth/tool transfer, real study/checkpoint, new-session resume/deepen and reset.

Do not assume GitHub pushes deploy Sites automatically. Never publish private checkpoints, source captures, runtime histories, caches or credentials as app code.

Current app scope needs no hosted connector for external research apps: agents browse through their own available tools. If connectors are later added, use current Sites eligibility/consent requirements first.

## 4. Full reset scope

The target is the entire scratchbook and all controlled runtime/checkpoint data, not just one visible report.

| State | Treatment |
| --- | --- |
| Studies/topics/source/cycle records and input queues | Purge all old-generation content |
| Findings, conversations, questions, results and hypotheses | Purge |
| Current and historical checkpoint objects, local Git history within them | Purge all controlled retained versions |
| Partial uploads, staged projections/imports, generated reports and exports | Purge controlled namespaces |
| Host canonical/operational data in reachable owned local runtime | Supported sandbox hard reset after session settlement |
| App-created backups/archives/content-bearing logs | Purge verified app-owned scope |
| Old worker authority/session token/head pointers | Invalidate before deletion |
| Browser app-controlled caches | Invalidate by generation; clear controlled entries |
| Reset receipt/epoch | Retain minimal content-free coordination |
| Application code, verified vendor packages, shipped purpose/profile | Preserve |
| Site identity, owner/admin binding and connection configuration | Preserve; revoke generation-specific credentials/authority |
| Detached/downloaded copies and platform/provider backups outside app control | Outside physical deletion control; report accurately |

GitHub files are not the default memory store. Removing files from Git history was one reason not to make same-source-repository storage the default. Full reset still must purge checkpoints containing local Git history; deleting only current D1 rows is insufficient.

## 5. Reset procedure and states

There is no single transaction across D1, R2 and local workspaces. Use a durable idempotent procedure with explicit component status:

1. **Authorize.** Admin sees target, generation, active sessions, deletion scope and destructive confirmation. Allocate a stable reset request ID.
2. **Fence.** Atomically advance reset generation, revoke active publishing authority and mark resetting. Reject old uploads/publications/continuations and new research starts for that generation.
3. **Settle reachable work.** Stop/cancel owned host work through supported lifecycle. A stale local agent cannot publish even if it ignores cancellation.
4. **Purge durable content.** Delete old-generation D1 research records and all R2 checkpoint/capture/report/staging versions in bounded pages. Repeat safely on partial failure.
5. **Clean local copies.** Connected agents acknowledge supported sandbox hard reset of exact app-owned roots. Track unreachable/detached session copies as pending/unknown; never pretend the Site can remotely erase a lost workspace.
6. **Prepare fresh state.** Leave an explicit empty new generation. A later authorized agent initializes a fresh guarded runtime from verified packages/default guidance; never restore a pre-reset checkpoint automatically.
7. **Verify.** Inspect authoritative coordinator/object inventories, old citation/export lookups and late publication rejection. Confirm reachable local cleanup where in scope.
8. **Report.** Durable purge completed and local-copy cleanup status are separate. Report full controlled cleanup only when required acknowledgments are present; never infer it from an empty UI.

The Site can fence and purge its durable scratchbook without starting an always-on host. It cannot guarantee physical deletion of disconnected execution disks, downloaded copies or external backups. Explain that limitation directly in Admin instead of making a false all-data-erased claim.

If durable deletion fails, remain reset_pending/failed with the precise store and retry action. If durable purge succeeds but connected local cleanup is missing, show that status explicitly. New generation work must not permit an old session to regain authority implicitly.

An identical old reset retry must not erase a newer generation's study. Old app imports, cached tabs, lease renewals and upload callbacks must carry generation/revision and fail after reset. Keep only content-free progress/receipts.

Local hard reset requires an explicitly disposable sandbox, canonicalized guarded root and supported host semantics. Refuse adopted/shared/external runtimes, symlinks/path traversal or instance/archive paths escaping scope. Never recursively delete an arbitrary user path.

## 6. Acceptance

- Owner/admin succeeds; anonymous/reader/forged-ID operations refuse.
- Pending requests and selected study/checkpoint survive browser restart/redeploy.
- Actual agent performs collection/analysis/cognition, checkpoint publication and fresh-session deepening.
- An old session losing fence/expiry/reset cannot publish a checkpoint or learning result.
- New input during a cycle survives conditional checkpoint/result publication.
- Partial upload, head-response loss, concurrent restore and reset have safe idempotent recovery.
- Durable purge removes old checkpoint versions/staging and content-bearing app state.
- Old tabs/citation/export/import cannot recover or resurrect cleared content.
- Reachable local copies are cleaned; unavailable-copy status is honest.
- Repeated reset/deploy does not delete or reseed newer data.
- Source/release/checkpoint artifacts contain no secrets, personal deployment IDs or private app data beyond the explicitly private runtime checkpoint payload.

Actual hosted transport, real inference, structural integrity, semantic review and physical cleanup claims are distinct evidence categories. Green builds or fixture rendering prove none of the others.
