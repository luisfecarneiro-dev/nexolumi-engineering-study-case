# 06 — Quality evidence and release criteria

**Reference snapshot:** October 11, 2026. Numbers below come from the author's private GitHub release notes and have **not** been independently rerun from this repository.

## Reported local quality evidence

| Test domain | Passing tests reported |
| --- | ---: |
| Frontend (Vitest) | 5,062 |
| Messaging service (Vitest) | 686 |
| Messaging gateway (Vitest) | 215 |
| Contracts (Vitest) | 203 |
| Observability | 12 |
| AI engine (pytest) | 146 |
| **Total passing** | **6,324** |

The frontend report additionally marked **one test skipped** and **one todo**. The composite local quality command reported exit code 0, type checking and a successful production frontend build. These are *reported outcomes from a private candidate release*, not publicly reproducible checks in this docs-only repository.

## Quality layers

| Layer | Purpose | Evidence policy |
| --- | --- | --- |
| Static checks | Catch type, lint and contract violations | Record actual command and outcome |
| Unit tests | Test isolated domain behavior | Avoid inferring external-provider success |
| Integration tests | Check coordinated modules and persistence | State fixture and environment assumptions |
| End-to-end tests | Exercise product workflows | Distinguish mocked from real provider flows |
| Security checks | Detect known boundary violations | Do not overstate assurance |
| Build validation | Detect packaging/runtime incompatibility | Record candidate version and environment |
| Runtime checks | Confirm readiness of deployed components | Prove per service, not just frontend |
| Soak / pilot | Exercise long-running and user-device behavior | Mandatory for final stability decision |

## Recorded release status

The private go/no-go document for the assessed release candidate concluded **NO-GO for Stable**. The documented reasons included an external CI gate awaiting rerun, an incomplete extended stability window, and pending real-world pilot validation. Later changes require a new evidence snapshot before the status can be updated.

This distinction matters in job interviews: I would rather explain a blocker and its remediation plan than claim a stable production release without adequate proof.

## How to interpret the result

- **Implemented:** source paths, schemas or features exist.
- **Locally tested:** deterministic tests passed in a specific environment.
- **Observed:** a particular runtime or provider interaction was checked and logged.
- **Released:** a change was deployed, which does not imply correctness in every case.
- **Stable:** all mandatory acceptance gates have been met and a final decision is recorded.

No screenshots of dashboards, confidential logs or detailed production metrics have been copied into the public package.

