# Upstream alignment and compatibility policy

Status: revised proposal. Mandatory for every implementation slice and future release.

## 1. Always aligned, reproducibly installed

Track the latest default-branch sources of host and harness, assess relevant contract/ownership changes and keep app adapters/prompts/specs/docs/fixtures synchronized. Install exact verified packages that the vendors support together.

Latest-source awareness does not authorize moving-branch production installs or package substitution based only on a version label.

## 2. Baseline reviewed on 24 November 2026

| Component | Source reviewed | Latest published package observed |
| --- | --- | --- |
| Harness | [f3248e17de50646484a525c951cf7e48c59efcd0](https://github.com/apenczolgolonabijorbiper/golem14-harness/commit/f3248e17de50646484a525c951cf7e48c59efcd0) | [v0.13.0](https://github.com/apenczolgolonabijorbiper/golem14-harness/releases/tag/v0.13.0) |
| Chelm | [cd14af43ddf79ef57719c65d8864e42513ed26df](https://github.com/apenczolgolonabijorbiper/chelm-host/commit/cd14af43ddf79ef57719c65d8864e42513ed26df) | [v1.3.0](https://github.com/apenczolgolonabijorbiper/chelm-host/releases/tag/v1.3.0) |

See [UPSTREAM_BASELINE.json](UPSTREAM_BASELINE.json). This is release-metadata/source observation, not a verified downloaded install lock.

Chelm 1.3.0 supports Harness 0.13.0 exactly and publishes JSON/Git provider selection. The earlier 0.12.0 unpublished-provider gate is obsolete. Host/provider integration API 2, provider contract 1, config/worker 3 and cognitive task 1 are separate negotiated dimensions.

Complete-runtime checkpoint/restore and durable session fencing remain new host work in [Chelm #45](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/45), slices #46–#48. Provider support does not imply checkpoint support. Hosted durable resume must wait for delivered/published compatible features and app acceptance.

Harness logical provider export/import is not a host checkpoint. Its ownership contract excludes host authorization/lease/operational lifecycle from portable knowledge. No Harness implementation issue is needed unless a concrete neutral gap is discovered.

Evidence:
- [Chelm compatibility](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/cd14af43ddf79ef57719c65d8864e42513ed26df/compatibility.json)
- [Chelm providers](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/cd14af43ddf79ef57719c65d8864e42513ed26df/docs/MEMORY_PROVIDERS.md)
- [Chelm runtime inventory](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/cd14af43ddf79ef57719c65d8864e42513ed26df/docs/RUNTIME.md)
- [Harness public boundary](https://github.com/apenczolgolonabijorbiper/golem14-harness/blob/f3248e17de50646484a525c951cf7e48c59efcd0/docs/MEMORY_PROVIDERS.md)

## 3. Review at task start, before merge and release

1. Resolve both current source heads and latest release/compatibility declarations.
2. Read relevant current public schemas/modules/prompts/install/lifecycle guidance and acceptance limitations at exact commits.
3. Compare with the last assessment; classify unaffected, adapter/prompt/profile/fixture change, packaging/checkpoint change or blocked vendor dependency.
4. Record exact commits, package pins/capabilities, affected areas, validation and any blocker/adoption issue. Generic “checked latest” is insufficient.
5. Update affected code/contracts, SudoLang specs/prompts, fixtures, dependency lock and docs together.
6. Recheck intervening upstream changes before merge/release.

Every app issue includes this requirement. Keep the app host/provider-neutral at semantic boundaries; never expose physical graph paths, copied validators or private module assumptions.

## 4. Planned drift automation

After approval, RN-02 adds pull-request/manual checks and a daily GitHub Actions drift report. It must report unknown/failed lookup honestly, avoid duplicate unchanged drift issues and never upgrade/reset/deploy production automatically.

A nonproduction compatibility lane may build exact source commits in isolated scratch runtimes. Source-only capability is not an official published release. The daily CI proposal is repository work, not a ChatGPT automation scheduled by this plan.

## 5. Exact lock and promotion

Record source URLs/commits reviewed, immutable release commit/tag, archive URL/name/hash, exhaustive manifest hash, required versions/capabilities, Node/Python/Git requirements and test evidence. Development source builds have separate identities.

Resolve the newest pair from Chelm declarations, never independently choose latest host and harness. Verify downloaded bytes and manifest before install. Unknown hashes stay unknown and block install/release.

The newest compatible supported published pair must be adopted before app release. Newer source-only changes need impact/compatibility evidence or a concrete blocker. If needed checkpoint capability is unpublished or unsupported, keep hosted resume acceptance blocked rather than bypassing it with an app ZIP script.

Promotion evidence includes clean install/upgrade; real intake/analysis/cognitive citation; checkpoint inventory/restore/fresh-session identity; session concurrency/fencing/reset; response-loss recovery; and actual affected client/hosted paths. Separate semantic verdicts from integrity checks.

Preserve existing knowledge during normal upgrades; settle active work and follow vendor procedures. Never use full reset as an upgrade/migration step. The default remains local Git with no remote; a new GitHub provider or optional SQLite is not a prerequisite.

Before Sites publication, recheck current runtime/auth/storage/build/MCP requirements and actual transfer capability. Never invent Site/plugin/tunnel IDs or assume a chat tool is a subprocess API.
