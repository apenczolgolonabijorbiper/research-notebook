# Research Notebook

Sample app for research studies assisted by Golem XIV in Chełm.

**Status: implementation plan awaiting owner review. The application is not implemented or published yet.**

Research Notebook will turn a user-provided subject into a small, source-grounded study: collect evidence, organize topics, inspect findings and uncertainty, and deepen selected questions through further cognition cycles.

It will use [Chelm-host](https://github.com/apenczolgolonabijorbiper/chelm-host) for runtime/lifecycle and [golem14-harness](https://github.com/apenczolgolonabijorbiper/golem14-harness) for cognitive procedure, memory semantics and validation. Cursor, Codex or ChatGPT Work supplies agent inference and available browsing tools.

## Review the proposal

- [Approval tracker #1](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/1)
- [Implementation plan](docs/IMPLEMENTATION_PLAN.md)
- [Upstream alignment policy](docs/UPSTREAM_ALIGNMENT.md)
- [Sites architecture and administrator full reset](docs/SITES_AND_RESET.md)
- [Observed upstream baseline](docs/UPSTREAM_BASELINE.json)

The tracker links twelve implementation slices with dependencies and acceptance criteria. Explicit owner approval is required before implementation starts. Reviewing or merging these planning documents alone does not authorize implementation.

## Proposed ways to use it

| Mode | Purpose | Where host+harness run |
| --- | --- | --- |
| Fixture notebook | Try navigation, evidence and study evolution immediately | No live execution; supplied data is labelled synthetic |
| Local agent | Real research with Cursor, Codex or Work execution tools | Verified packages in a private, persistent local/runtime environment |
| Connected Sites scratchbook | Publish a usable notebook through ChatGPT Work Sites | Separate persistent Chelm runtime reached through a scoped application gateway |

Sites runs Cloudflare Worker code; it cannot directly execute the current Python/Git-based runtime. The hosted UI will use D1/R2 for application records and files. Canonical cognitive memory stays under Chelm ownership.

## Requirements proposed for v1

- Bounded collection, traceable captured sources and grounded cognitive results.
- Topics/chapters, linked follow-ups, conflicts, hypotheses and changes between cycles.
- Responsive notebook, evidence inspector and HTML/Markdown/JSON report exports.
- Saved setup/reconnect instructions and SudoLang workflows for all three agent surfaces.
- Owner/admin full reset across controlled notebook and sandbox runtime data, with old work fenced out.
- Continuous review of latest upstream sources and exact, tested production dependency pins.
- Actual client and connected Sites acceptance evidence before general support claims.

These are planned requirements, not existing features. No model endpoint/key is mandatory for agent-driven operation; each live route still needs actual executor, browsing/tool and persistence access.
