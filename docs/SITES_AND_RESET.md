# Sites deployment and scratchbook full reset

Status: proposed for owner review. This document defines requirements; no Site, secret, runtime or reset has been created by this plan.

## 1. Hosting decision

The published app is a scratchbook, with persistent data until its administrator chooses full reset.

Sites currently runs Cloudflare Workers (128 MB per isolate, HTTP-based external connections, no raw TCP). The present upstream engine needs Node 22.12+, Python 3.12+ and Git. A Worker cannot spawn that toolchain or use a durable local Git checkout.

Therefore:

- The browser renders the shared notebook UI.
- A Sites Worker handles authenticated application routes and app persistence.
- D1 stores scratchbook/study/topic/settings/projection/reset records.
- R2 stores app-owned source captures, import/export files and generated reports.
- A scoped HTTPS gateway invokes public Chelm operations on a persistent owner-controlled machine.
- Chelm owns the canonical runtime and its provider; Harness supplies cognition/validation semantics.
- Cursor, Codex or Work supplies actual agent inference and available browsing. An executor still has to be connected/invoked; publishing does not turn a chat session into a background service.

This design is compatible with easy publication of the frontend/application layer. A real hosted research workflow additionally needs the reachable persistent runtime. Fixture mode demonstrates UI/evidence navigation immediately and is visibly labelled synthetic.

Do not promise that a static import or browser-only mode executes host+harness. Source-only provider abstraction does not, by itself, make Chelm or its current JSON/Git backend Worker-compatible. D1 in this app is not the optional upstream SQLite memory provider.

## 2. Runtime boundary and connection

The gateway is a narrow app-owned adapter, not a second delegated-work architecture.

Allowlisted operations cover capability/status, authorized intake, task create/get/list/continue/cancel/evidence/learning, projection refresh and reset coordination. Each maps to public upstream APIs. Worker execution continues through Chelm's existing worker transports. Never expose arbitrary shell, file, SQL, module or target-URL execution.

On every call, validate caller/service scope, notebook binding, generation, operation/input bounds and idempotency key. A browser cannot pick a runtime path, principal or provider implementation. A user identity header copied into a public gateway request is not trusted; authenticate the server-to-server channel and map it to fixed authorized scopes.

Use HTTPS from Sites to a reachable gateway. Private tunnel binding is an optional deployment route only if selected account/platform support is observed. Browser code never receives gateway/admin/worker credentials. Configure hosted values in Sites runtime settings and local values outside Git.

For local use the same interface calls Chelm through the local CLI. MCP remains optional for that route.

## 3. Identity and administration

Start with an owner-private Site. Sites provides signed-in request identity; its user ID is scoped to the Site. Identity-dependent pages/routes must use supported server-side auth handling. Missing identity, a public page or possession of a notebook ID does not confer admin rights.

Set administrator IDs through deployment/operator configuration, not through “first visitor becomes admin.” Authorize each reset route/tool on the server. Hide the Admin UI for ordinary readers, but server-side refusal is the control.

Use same-origin POST for mutations, validate origin/CSRF as appropriate, and apply duplicate-submission protection. Do not reset on render, GET, startup, import, migration, deploy, prefetch or reconnect. No reset credential belongs in a URL, source bundle or generated report.

If optional Site MCP operations are implemented, use the platform-supported endpoint/discovery/auth flow and the plugin created by publishing. A view permission is not an admin permission. Preserve per-operation owner/admin checks; tools cannot select an arbitrary notebook/runtime.

## 4. Persistence and source publishing

The GitHub repository is the authoritative app source. Sites may maintain a distinct managed source repository; do not assume pushing GitHub master deploys a Site.

Proposed publication procedure:

1. Select the reviewed GitHub commit and produce a sanitized application source snapshot.
2. Reuse an existing Site identity or create one only during authorized implementation/publication.
3. Write the returned project ID exactly into the deployment checkout's .openai/hosting.json; publishable templates contain no fabricated or personal hardcoded Site ID.
4. Configure logical D1/R2 bindings, owner/admin identity and needed runtime values using supported tools.
5. Generate and review bounded schema-only D1 migrations; keep seed/fixture loading separate and explicit.
6. Build a Worker ES module at dist/server/index.js with a default fetch handler and its UI assets; do not mark this connected application as static-only.
7. Validate, synchronize/package exact source, save that version and deploy through Sites, preserving its audience.
8. Record the GitHub commit, Sites source commit, archive identity, saved version and deployment status/URL.
9. Verify authenticated notebook access, durable app data, connected runtime task/citation behavior and admin refusal/reset.

A Site-specific source checkout may track its actual identity as required by Sites; do not commit personal deployment identity into the reusable public sample. Never copy private runtime, credentials, Git history of studies, dependency caches or raw local setup state into that source snapshot.

Ordinary redeploys retain D1/R2 data and runtime bindings. Browser storage contains preferences only; study truth must survive device/session changes. Fixture loading is an explicit user action and never a migration/redeploy seed that repopulates cleared data.

The first release does not depend on Sites calling other connected apps from browser/server code. Agents use their own browsing tools. If future connector actions are added, first perform current Sites eligibility/discovery and consent checks; native tool availability alone does not prove hosted eligibility.

## 5. Full reset scope

Full reset targets the entire scratchbook and its dedicated app-owned sandbox runtime. Optional per-study deletion is a separate later feature; do not call it full reset.

| Store or state | Full reset treatment |
| --- | --- |
| Study/topic/source/cycle records and app projections | Delete all old-generation content |
| Conversations, user replies, result text and cognitive questions | Delete |
| Canonical observations/evidence, ledger, entities/claims, hypotheses and queues | Reset through supported host sandbox lifecycle |
| Canonical Git history/provider snapshots and retained source citations | Delete within the dedicated owned reset boundary |
| Host assignments, staging, task results, receipts with research content and pending learning | Invalidate and remove through host-supported reset/cleanup |
| App captures, attachments, imported bundles, generated HTML/Markdown/JSON exports | Purge app-controlled file/object namespaces |
| App-created reset archives, backups and content-bearing logs | Purge only those inside verified app-owned scope |
| Browser caches/preferences that contain old notebook data | Invalidate by generation; clear this app's controlled entries |
| Reset control record | Keep minimal content-free request ID/generation/completion state |
| App source, vendor packages and shipped purpose/profile | Preserve |
| Site identity, deployment, administrator binding and configured connection secrets | Preserve; revocable generation-specific worker tokens must be replaced/invalidated |
| Downloaded copies, screenshots, external exports and platform/provider backups outside app control | Outside reset scope; no secure physical-erasure claim |

A report export is not a runtime backup. If app runtime backup is introduced, it must be in the deletion inventory; backup retention cannot silently undermine scratchbook reset.

Use the supported host hard-reset operation only on a runtime explicitly initialized resettable, with the required sandbox guard. An adopted external/shared runtime is never eligible automatically. Ordinary reset that archives memory fails this full-clear requirement.

## 6. Resumable reset protocol

There is no cross-system atomic transaction spanning D1, R2 and the external host. Implement a durable, retryable procedure with explicit acknowledgments.

1. **Authorize and identify.** Admin sees scratchbook/runtime target, active work, scope and a clear destructive confirmation. Assign a reset request ID; identical retries use it.
2. **Fence.** Advance/invalidate the notebook generation durably and mark resetting. Reject old-generation reads/writes and new research operations. Acquire the host-side fence before permitting any new assignment for the next generation.
3. **Cancel and settle.** Cancel tasks, abandon/drain assignments and release owned work through supported host APIs. Reject late result/learning publication. Wait or refuse safely if upstream ownership rules prevent settlement.
4. **Clear canonical runtime.** Verify sentinel, real paths, disposable ownership and absence of paths escaping scope; invoke supported sandbox hard reset. Ensure old runtime/assignment identities cannot authorize the new generation.
5. **Purge app stores.** Delete old-generation D1 rows and R2 objects in bounded pages; remove app-owned generated files/logs/archives. Maintain content-free progress.
6. **Reinitialize.** Recreate a fresh app-owned runtime with verified packages and shipped default purpose/profile, preserving deployment/admin/connection configuration.
7. **Verify.** Query authoritative host status and controlled storage inventories. Confirm zero previous study content, unavailable old citations, rejected old work and a clean empty notebook.
8. **Complete.** Mark success only after both host and app stores acknowledge completion; keep a minimal reset receipt and new generation.

All steps must be restart-safe and idempotent. A host timeout is not proof that reset failed or succeeded: query authoritative state with the same request ID. If any controlled store cannot be cleared, remain reset_pending/failed with the exact remaining component and a retry path. The empty UI alone is never a success criterion.

Readers must stop serving the old generation once the fence is active, even if physical cleanup is pending. Destructive completion remains pending until actual deletion acknowledgments are obtained.

Generation enforcement includes direct executor paths, not just browser routes. Revoke/version per-generation remote endpoint binding or worker credentials so an old agent cannot silently attach to the new runtime after hard reset. If vendor APIs cannot guarantee this, RN-10 must record an upstream blocker and cannot claim acceptance; do not patch private vendor internals.

Do not delete beyond the exact ownership root. Resolve/canonicalize paths and refuse symlinks, traversal or instance/archive locations that escape it. Never recursively remove a user-selected arbitrary directory.

## 7. Required hosted/reset acceptance

Use disposable synthetic studies, then verify a real source-backed study separately.

- Owner succeeds; anonymous, reader and forged-ID requests fail.
- D1/R2 and canonical host state survive a normal deployment/reconnect.
- A live published study processes sources, answers with retained citations and deepens on new evidence.
- Reset while a worker is running prevents every late publication and learning attempt.
- Old browser tabs and queued continuations cannot resurrect cleared content.
- Gateway down, active lease, partial object deletion, response loss and process restart have honest states and safe retry.
- A completed reset leaves no prior content in controlled search/export/citation/runtime/artifact views.
- Repeated reset requests do not delete a newly created study under a later generation.
- Redeploy and old import callbacks cannot silently restore previous data.
- External/shared/non-sandbox roots and cross-owner targets are refused.
- Content-free reset records may remain; no content-bearing backup is hidden from the deletion inventory.

Actual hosted observations, structural checks and semantic review must be recorded separately. Fixture rendering or a green build proves neither live inference nor full reset.
