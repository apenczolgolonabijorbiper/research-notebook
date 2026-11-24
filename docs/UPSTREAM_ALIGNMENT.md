# Upstream alignment and compatibility policy

Status: proposed, awaiting owner approval. Applies to every implementation issue and every later release.

## Meaning of “always aligned”

Research Notebook must track the latest sources of both upstream projects and keep its app/host/harness responsibility boundaries, prompts, contracts and examples in sync. It must also remain reproducible: production runs exact verified packages that their vendors support together.

“Latest” does not authorize moving-branch installs, unpublished production features, a version-only compatibility check, or silent replacement of an existing archive. Source awareness and released-package adoption are separate gates.

## Baseline observed on 24 November 2026

| Component | Observed source | Observed released package |
| --- | --- | --- |
| golem14-harness | [00aba14b49641c3deb9d17d7cc327f6f8578f503](https://github.com/apenczolgolonabijorbiper/golem14-harness/commit/00aba14b49641c3deb9d17d7cc327f6f8578f503) | [v0.12.0](https://github.com/apenczolgolonabijorbiper/golem14-harness/releases/tag/v0.12.0) |
| chelm-host | [8a0e533c6689f589dba1ec00dd82397d403b3a0b](https://github.com/apenczolgolonabijorbiper/chelm-host/commit/8a0e533c6689f589dba1ec00dd82397d403b3a0b) | [v1.2.0](https://github.com/apenczolgolonabijorbiper/chelm-host/releases/tag/v1.2.0) |

See [UPSTREAM_BASELINE.json](UPSTREAM_BASELINE.json). This is a planning observation from repository files and release metadata, not a completed package-install verification or a permanent lockfile.

Important current difference:

- Published Chelm 1.2.0 supports the published Harness 0.12.0 exactly.
- Latest Harness source adds the public provider/semantic-change architecture.
- Latest Chelm source integrates it through opt-in JSON/Git provider selection and memory worker v3.
- Chelm marks that path development-build-unpublished and production-disabled, with a separate exact development pin. That package still has VERSION 0.12.0 but differs from the published archive.
- App design must respect the new provider ownership boundary immediately. Production adoption waits for a supported published pair and packaged acceptance, not a local override.

Source evidence:
- [Chelm compatibility declaration](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/8a0e533c6689f589dba1ec00dd82397d403b3a0b/compatibility.json)
- [Chelm development package pin](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/8a0e533c6689f589dba1ec00dd82397d403b3a0b/provider-compatibility.json)
- [Chelm provider integration](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/8a0e533c6689f589dba1ec00dd82397d403b3a0b/docs/MEMORY_PROVIDERS.md)
- [Chelm provider acceptance/gates](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/8a0e533c6689f589dba1ec00dd82397d403b3a0b/docs/MEMORY_PROVIDER_ACCEPTANCE.md)
- [Harness public memory boundary](https://github.com/apenczolgolonabijorbiper/golem14-harness/blob/00aba14b49641c3deb9d17d7cc327f6f8578f503/docs/MEMORY_PROVIDERS.md)
- [Harness cognition contract](https://github.com/apenczolgolonabijorbiper/golem14-harness/blob/00aba14b49641c3deb9d17d7cc327f6f8578f503/docs/COGNITIVE_TASKS.md)
- [Chelm cognition binding](https://github.com/apenczolgolonabijorbiper/chelm-host/blob/8a0e533c6689f589dba1ec00dd82397d403b3a0b/docs/COGNITIVE_TASKS.md)

## Required check at the beginning of every issue

1. Resolve both default branches to exact commits and read current release/compatibility declarations.
2. Read relevant public contracts, schemas, prompts, install/update/lifecycle guidance and acceptance limitations at those commits.
3. Compare against the last recorded review. Classify the change as unaffected, adapter change, prompt/profile/fixture change, packaging change, or blocked by a vendor contract/release.
4. Record the impact and intended adoption in the issue/PR. A generic “checked latest” without commits and reasoning is insufficient.
5. Update affected app contracts, adapters, fixture data, SudoLang prompts, source/spec examples, docs and tests in the same change.
6. Re-resolve before merge and release; review any newly intervening relevant upstream change.

Required review fields: checked date/time, source commit for each project, latest release identity, current runtime pin, required/observed capabilities, affected app areas, validation evidence, remaining incompatibilities and follow-up link.

## Planned automated drift checks

Implement these only after plan approval in RN-02:

- Pull-request and manually dispatched checks compare lock/contract fixtures to current upstream.
- Daily GitHub Actions check reports source/release drift; avoid duplicate issues for an unchanged upstream pair.
- A development compatibility lane can build exact upstream commits in isolated nonproduction scratch runtimes.
- Failure or unavailable upstream access is “unknown/blocked,” never a false current result.
- No automatic runtime upgrade, production reset, dependency-pin mutation or deployment.

The daily CI check is part of the proposed repository implementation, not a ChatGPT automation being scheduled by this plan.

## Dependency lock to implement

Record each upstream source URL and reviewed commit; published tag and immutable release commit; package archive name/URL/hash and manifest hash; contract/capability versions; runtime requirements; test matrix; and verification evidence. For development lanes, record exact source build/manifest identities separately.

Resolve the newest supported compatible pair through Chelm's declarations, then verify downloaded archive bytes and exhaustive release manifest before install. Never select the newest versions of host and harness independently.

Unknown hashes remain unknown and block install/release until verified. Release metadata digests in the planning baseline must be independently checked against downloaded bytes during bootstrap/release work.

## Production and release rules

- The newest supported compatible published pair must be adopted before an app release. A newer compatible pair makes the release gate fail until adapters, lock, prompts/docs and acceptance are updated.
- More recent source-only changes must have impact assessment and isolated compatibility evidence, or a concrete upstream blocker. Do not hide source drift behind an older release pin.
- If a needed feature exists only in a production-disabled upstream path, the app feature stays blocked. Document the vendor dependency; do not modify vendor packages or label a source build as an official release.
- Never expose canonical graph files or private module assumptions as an app contract. Optional physical provider choice belongs to Chelm.
- Recheck Sites runtime/build/auth/storage/MCP requirements as well as both GitHub projects before publication. Do not invent a Site ID, plugin ID or tunnel capability.
- Upgrades pause/settle work, use vendor-supported procedures, preserve knowledge and validate before resume. A normal upgrade must never invoke scratchbook full reset.
- Keep fixtures and SudoLang specifications synchronized with code changes. The app must follow future vendor language-neutral contracts when published; the currently planned upstream portability work is not claimed as delivered.

## Promotion evidence

A dependency promotion includes: package identities and verified hashes; negotiated versions; clean install/upgrade; real source intake/analysis/cognitive result/citation reads; learning if enabled; restart/recovery; generation/reset refusal cases; client/hosting routes affected; and separate semantic review of actual model results.

Do not widen testing after sufficient unchanged checks merely for volume. Select checks by the change's actual contracts and unresolved risks.
