# Research Notebook

Sample app for research studies assisted by Golem XIV in Chełm.

**Status: revised implementation plan awaiting owner approval to implement. No runnable application or Site has been delivered yet.**

Research Notebook will collect a bounded set of sources for a user-provided subject, organize evidence-backed findings into topics, expose uncertainty and follow-up questions, and let the user deepen or challenge the study.

## Review

- [Approval tracker #1](https://github.com/apenczolgolonabijorbiper/research-notebook/issues/1)
- [Planning PR #14](https://github.com/apenczolgolonabijorbiper/research-notebook/pull/14)
- [Implementation plan](docs/IMPLEMENTATION_PLAN.md)
- [Runtime checkpoints and agent sessions](docs/RUNTIME_CHECKPOINTS.md)
- [Sites publication and admin reset](docs/SITES_AND_RESET.md)
- [Upstream alignment policy](docs/UPSTREAM_ALIGNMENT.md)
- [Dated upstream baseline](docs/UPSTREAM_BASELINE.json)

## Primary architecture

**Sites + active agent + local Git + private durable checkpoints.**

Cursor, Codex or ChatGPT Work runs the real [Chelm-host](https://github.com/apenczolgolonabijorbiper/chelm-host) and [Harness](https://github.com/apenczolgolonabijorbiper/golem14-harness) locally. Memory lives in its own local Git repository, outside application source. GitHub is not required for runtime memory storage. A dedicated host server is optional.

For temporary Work environments, a validated complete-runtime checkpoint in private storage makes later-session resume possible. Sites D1 holds requests, notebook views and coordinator metadata; R2 holds source/report files and checkpoints. Publishing a Site does not start inference. An authorized agent must explicitly connect and run bounded work.

| Mode | Execution | Persistence |
| --- | --- | --- |
| Fixture | No live cognition; synthetic sample data | Local app data or Sites storage |
| Persistent local agent | Local Chelm + Harness + Git | Private local runtime; optional backup |
| Sites with temporary agent | Chelm + Harness in Work/Codex/Cursor execution environment | Private verified runtime checkpoints plus D1/R2 app state |

## Planned capability

Grounded research, topics/chapters, linked questions, hypotheses/conflicts, revised findings, source inspection, HTML/Markdown/JSON reports, three-client SudoLang workflows and administrator full reset.

The current published baseline is **Chelm 1.3.0 / Harness 0.13.0**, which includes the provider architecture. Complete-runtime checkpoint/resume and durable session fencing remain **new work**, tracked in [Chelm #45](https://github.com/apenczolgolonabijorbiper/chelm-host/issues/45) and slices #46–#48. Hosted durable-resume claims are blocked until that capability is delivered, published and tested.

GitHub memory backup and an always-on gateway are optional later integrations. Persona1 supplies useful design precedent; its private history/data and older custom adapter are not copied into this sample.

All app issues have dependencies and acceptance criteria. This revision changes planning only; implementation, deployment, schedules and resets have not been authorized by it.
